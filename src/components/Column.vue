<template>
  <div class="column">
    <header>
      <h3>{{ name }}</h3>
    </header>
    <Card v-for="card in cards" :key="card.id" :card="card"></Card>
  </div>
</template>

<script>
  import Card from './Card.vue'

  export default {
    name: 'Column',
    components: { Card },
    props: {
      name,
      cards: null
    },
}
</script>

<style scoped>
  .column {
    flex: 30%;
    padding: 5px;
    height: 400px;
    background-color: lightgrey;
  }
</style>
