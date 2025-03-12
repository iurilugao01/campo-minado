<script>
import { ref, onMounted } from "vue";

export default {
  name: "MineField",
  props: {
    fieldSize: Number,
  },
  setup(props) {
    const isBomb = ref([]);
    const isField = ref([]);
    const isClicked = ref([]);
    const isBanner = ref([]);

    let markBanner = ref(false);
    let isWinner = ref(false);

    const findField = (array, location) =>
      array.some((field) => field.x === location.x && field.y === location.y);

    const randomizeFields = () => {
      for (let i = 1; i <= props.fieldSize; i++) {
        for (let j = 1; j <= props.fieldSize; j++) {
          const result = Math.random() < 0.5;
          const location = { x: j, y: i };
          result ? isBomb.value.push(location) : isField.value.push(location);
        }
      }
    };

    const clickField = (location) => {
      if (isWinner.value) return;
      if (markBanner.value) {
        clickBanner(location);
        return;
      }
      if (findField(isBanner.value, location)) return;
      if (findField(isBomb.value, location)) {
        alert("KABOOM");
        window.location.reload();
      }
      if (!findField(isClicked.value, location)) isClicked.value.push(location);
      if (isField.value.every((el) => findField(isClicked.value, el))) {
        alert("Parabens! Você venceu! jogo encerrado");
        isWinner.value = true;
      }
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

    onMounted(() => {
      randomizeFields();
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
      isWinner,
    };
  },
};
</script>

<template>
  <table>
    <tr>
      <th @click="markBanner = !markBanner">
        <i :class="['bi', markBanner ? 'bi-flag-fill' : 'bi-flag']"></i>
      </th>
      <th v-for="header in fieldSize" :key="header">{{ header }}</th>
    </tr>
    <tr v-for="line in fieldSize" :key="line">
      <th>{{ line }}</th>
      <td
        v-for="block in fieldSize"
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
