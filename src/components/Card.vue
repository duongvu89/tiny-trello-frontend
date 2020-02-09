<template>
  <div class="card">
      <p v-if="card.description"> {{ card.description }}</p>
  </div>
</template>

<script>
  export default {
    name: 'Card',
    props: {
      card: Object
    },
  }
</script>

<style scoped>
  .card {
    display: flex;
    border-radius: 3px;
    cursor: default;
    background-color: white;
    margin: 5px;
    padding: 5px;
    color: #172b4d;
  }
</style>
