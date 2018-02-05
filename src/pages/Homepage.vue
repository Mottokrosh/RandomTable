<template>
  <div>
    <random-table full-width
      :labels="labels"
      :rows="rows"
    ></random-table>

    <random-table
      :labels="labels2"
      :rows="rows2"
    ></random-table>
  </div>
</template>

<script>
  import RandomTable from '../components/RandomTable';

  export default {
    components: {
      RandomTable,
    },

    data() {
      return {
        labels: ['Thing', 'Notes'],
        rows: [
          ['Sugar', '-'],
          ['Salt', 'Coarse'],
          ['Pepper', 'Mixed'],
          ['Chili', ''],
          ['Paprika', 'Thing'],
          ['Garlic', ''],
          ['Onion', 'Sliced'],
        ],
        labels2: ['Item'],
        rows2: [
          ['Hammer'],
          ['Axe'],
          ['Morningstar'],
        ],
      }
    },
  }
</script>

<style>

</style>
