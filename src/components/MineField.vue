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
    let markBanner = false;

    const findField = (array, location) => {
      return array.some(
        (field) => field.x === location.x && field.y === location.y
      );
    };

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
      if (markBanner) {
        if (!findField(isClicked.value, location)) {
          isClicked.value.push(location);
        } else {
          console.log("armazenado");
        }
        return;
      }
      if (findField(isBomb.value, location)) {
        alert("KABOOM");
        window.location.reload();
      } else {
        if (!findField(isClicked.value, location)) {
          isClicked.value.push(location);
        } else {
          console.log("armazenado");
        }
      }
    };

    const showNearBombs = (location) => {
      if (markBanner) {
        return "!";
      }
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
        if (findField(isBomb.value, el)) {
          nearBombs++;
        }
      });
      return nearBombs;
    };

    const ToggleMarkBanner = () => (markBanner = !markBanner);

    onMounted(() => {
      randomizeFields();
      console.log(isBomb.value);
    });

    return {
      isBomb,
      isField,
      isClicked,
      findField,
      clickField,
      showNearBombs,
      ToggleMarkBanner,
    };
  },
};
</script>

<template>
  <table>
    <tr>
      <th @click="ToggleMarkBanner()">!</th>
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
