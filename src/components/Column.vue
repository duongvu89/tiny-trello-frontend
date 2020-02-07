<template>
  <div :status="status" class="column">
    <header>
      <h3>{{ name }}</h3>
    </header>
    <draggable @end="onEnd" group="cards" v-model="columnCards">
      <Card :card="card" :key="card.id" :status="card.status" v-for="card in columnCards"></Card>
    </draggable>
  </div>
</template>

<script>
  import Card from './Card.vue'
  import draggable from "vuedraggable";

  export default {
    name: 'Column',
    components: {Card, draggable},
    props: {
      name,
      cards: null,
      status: null,
    },
    data() {
      return {
        columnCards: null
      }
    },
    mounted() {
      this.columnCards = [...this.cards];
      console.log(this.columnCards);
    },
    methods: {
      onEnd: function (evt) {
        console.log(evt.to.parentNode.getAttributeNode("status").value);

      },
    }
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
