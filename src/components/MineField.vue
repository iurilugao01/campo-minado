<script>
import { ref, onMounted } from "vue";

export default {
  name: "MineField",
  setup() {
    const isBomb = ref([]);
    const isField = ref([]);
    const isClicked = ref([]);
    const isBanner = ref([]);
    const markBanner = ref(false);
    const specialField = ref();
    const level = ref(2);

    const findField = (array, location) =>
      array.some((field) => field.x === location.x && field.y === location.y);

    const randomizeFields = () => {
      for (let i = 1; i <= level.value; i++) {
        for (let j = 1; j <= level.value; j++) {
          const result = Math.random() < 0.5;
          const location = { x: j, y: i };
          result ? isBomb.value.push(location) : isField.value.push(location);
        }
      }
    };

    const randomizeSpecialFIeld = () => {
      const index = Math.floor(Math.random() * isField.value.length);
      specialField.value = isField.value[index];
    };

    const clickField = (location) => {
      if (markBanner.value) {
        clickBanner(location);
        checkWinned();
        return;
      }
      if (findField(isBanner.value, location)) return;
      if (findField(isBomb.value, location)) {
        alert("KABOOM");
        resetLevel();
        return;
      }
      if (!findField(isClicked.value, location)) isClicked.value.push(location);
      checkWinned();
    };

    const showNearBombs = (location) => {
      const nearFields = [
        { x: location.x, y: location.y + 1 },
        { x: location.x + 1, y: location.y + 1 },
        { x: location.x + 1, y: location.y },
        { x: location.x + 1, y: location.y - 1 },
        { x: location.x, y: location.y - 1 },
        { x: location.x - 1, y: location.y - 1 },
        { x: location.x - 1, y: location.y },
        { x: location.x - 1, y: location.y + 1 },
      ];
      let nearBombs = 0;
      nearFields.forEach((el) => {
        if (findField(isBomb.value, el)) nearBombs++;
      });
      return nearBombs;
    };

    const clickBanner = (location) => {
      if (findField(isClicked.value, location)) return;
      if (findField(isBanner.value, location)) {
        const index = isBanner.value.findIndex(
          (item) => item.x === location.x && item.y === location.y
        );
        isBanner.value.splice(index, 1);
        return;
      }
      isBanner.value.push(location);
    };

    const resetLevel = () => {
      console.log(isField.value);

      isBomb.value = [];
      isField.value = [];
      isClicked.value = [];
      isBanner.value = [];

      markBanner.value = false;
      randomizeFields();
      randomizeSpecialFIeld();
    };

    const checkWinned = () => {
      const allFieldsClicked = isField.value.every((el) =>
        findField(isClicked.value, el)
      );
      const allBombsMarked = isBomb.value.every((el) =>
        findField(isBanner.value, el)
      );
      if (allFieldsClicked && allBombsMarked) {
        level.value++;
        resetLevel();
      }
    };

    onMounted(() => {
      randomizeFields();
      randomizeSpecialFIeld();
    });

    return {
      isBomb,
      isField,
      isClicked,
      isBanner,
      findField,
      clickField,
      showNearBombs,
      markBanner,
      specialField,
      level,
    };
  },
};
</script>

<template>
  <h3>Nivel atual: {{ level - 1 }}</h3>
  <table>
    <tr>
      <th @click="markBanner = !markBanner">
        <i :class="['bi', markBanner ? 'bi-flag-fill' : 'bi-flag']"></i>
      </th>
      <th v-for="header in level" :key="header">{{ header }}</th>
    </tr>
    <tr v-for="line in level" :key="line">
      <th>{{ line }}</th>
      <td
        v-for="block in level"
        :key="`${block}-${line}`"
        @click="clickField({ x: block, y: line })"
      >
        <span v-if="findField(isClicked, { x: block, y: line })">
          {{ showNearBombs({ x: block, y: line }) }}
        </span>
        <span v-if="findField(isBanner, { x: block, y: line })">
          <i class="bi bi-flag"></i>
        </span>
      </td>
    </tr>
  </table>
</template>

<style>
body {
  background-color: #000;
}

table {
  background-color: #fff;
}

th,
td {
  width: 20px;
  height: 20px;

  cursor: pointer;
}

th {
  background-color: #454545;
}

td {
  background-color: #0e0e0e;
  text-align: center;
}
</style>
