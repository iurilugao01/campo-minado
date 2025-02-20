<script>
export default {
  name: "GlobalField",
  data() {
    return {
      isBomb: [],
      isField: [],
    };
  },
  props: {
    fieldSize: Number,
  },
  methods: {
    randomizeFields() {
      for (let i = 0; i < this.fieldSize; i++) {
        for (let j = 0; j < this.fieldSize; j++) {
          const result = Math.random() < 0.5;
          const location = { x: j, y: i };
          result ? this.isBomb.push(location) : this.isField.push(location);
        }
      }
    },
    clickField(location) {
      if (
        this.isBomb.some(
          (bomb) => bomb.x === location.x && bomb.y === location.y
        )
      ) {
        return alert("KABOOM");
      }
      alert("nao explodiu :D");
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
      ></td>
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
}
</style>
