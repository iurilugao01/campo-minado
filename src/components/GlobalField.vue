<script>
export default {
  name: "GlobalField",
  data() {
    return {
      isBomb: [],
      isField: [],
      isClicked: [],

      gameOver: false,
    };
  },
  props: {
    fieldSize: Number,
  },
  methods: {
    findField(array, location) {
      return array.some(
        (field) => field.x === location.x && field.y === location.y
      );
    },
    randomizeFields() {
      for (let i = 1; i <= this.fieldSize; i++) {
        for (let j = 1; j <= this.fieldSize; j++) {
          const result = Math.random() < 0.5;
          const location = { x: j, y: i };
          result ? this.isBomb.push(location) : this.isField.push(location);
        }
      }
    },
    clickField(location) {
      if (this.findField(this.isBomb, location)) {
        this.gameOver = true;
        alert("KABOOM");
      } else {
        !this.findField(this.isClicked, location)
          ? this.isClicked.push(location)
          : console.log("tu ja clicou ai krl");
      }
    },
    showNearBombs(location) {
      const nearFields = [];
      let nearBombs = 0;

      nearFields.push(
        { x: location.x, y: location.y + 1 },
        { x: location.x + 1, y: location.y + 1 },
        { x: location.x + 1, y: location.y },
        { x: location.x + 1, y: location.y - 1 },
        { x: location.x, y: location.y - 1 },
        { x: location.x - 1, y: location.y - 1 },
        { x: location.x - 1, y: location.y },
        { x: location.x - 1, y: location.y + 1 }
      );
      nearFields.forEach((el) => {
        if (this.findField(this.isBomb, el)) {
          nearBombs++;
        }
      });
      return nearBombs;
    },
  },

  mounted() {
    this.randomizeFields();
    console.log(this.isBomb);
  },
};
</script>

<template>
  <table>
    <tr>
      <th>0</th>
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
