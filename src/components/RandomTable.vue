<template>
  <table :class="classes">
    <thead>
      <tr>
        <th :colspan="labels.length + 1">
          <button @click.prevent="roll">Roll</button>
          <button @click.prevent="clear">Clear</button>
        </th>
      </tr>
      <tr>
        <th class="centered">{{ dieType }}</th>
        <th v-for="(label, index) in labels" :key="index">
          {{ label }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="(row, rowIndex) in rows"
        :class="{ active: rowIndex === activeRow }"
      >
        <td class="centered">{{ rowIndex + 1 }}</td>
        <td v-for="(col, colIndex) in row" :key="colIndex">
          {{ col }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
  export default {
    props: {
      labels: Array,
      rows: Array,
      fullWidth: Boolean,
    },

    data() {
      return {
        activeRow: null,
      };
    },

    computed: {
      dieType() {
        return `d${this.rows.length}`;
      },

      classes() {
        return {
          'mottokrosh-random-table': true,
          'full-width': this.fullWidth,
        };
      }
    },

    methods: {
      roll() {
        const cycles = this.rows.length > 12 ? 1 : ( this.rows.length > 6 ? 2 : 3 );
        const rnd = this.randomRow();
        const steps = cycles * this.rows.length + rnd;
        const delays = [];
        let timeout = 0;

        for (let i = steps -1 ; i >= 0; i--) {
          delays[i] = 100;
        }

        if (delays[delays.length - 1]) delays[delays.length - 1] = 500;
        if (delays[delays.length - 2]) delays[delays.length - 2] = 320;
        if (delays[delays.length - 3]) delays[delays.length - 3] = 200;

        for (let c = 0; c < steps; c++) {
          timeout = timeout + delays[c];
          setTimeout(() => {
            this.activeRow = c % this.rows.length;
          }, timeout);
        }
      },

      clear() {
        this.activeRow = null;
      },

      randomRow() {
        return Math.floor(Math.random() * Math.floor(this.rows.length));
      },
    },
  }
</script>

<style scoped>
  table {
    text-align: left;
    margin: 0 0 1em 0;
    border-spacing: 0 2px;
    border-collapse: separate;
  }
  th {
    background-color: #e4e4e4;
    padding: 4px 8px;
    font-weight: bold;
  }
  td {
    background-color: #f4f4f4;
    padding: 4px 8px;
    transition: all 100ms linear;
  }
  button {
    border: none;;
    border-radius: 3px;
    background-color: #eb5026;
    -webkit-appearance: none;
    appearance: none;
    padding: 4px 8px;
    color: #fff;
    transition: all 100ms linear;
  }
  button:hover {
    background-color: #ff5729;
  }
  .active {
    box-shadow: 0 0 3px rgba(0, 0, 0, 0.5);
  }
  .active td {
    background-color: #abcfde;
    transform: scale(1.1);
  }
  .centered {
    text-align: center;
  }
  .full-width {
    width: 100%;
  }
</style>
