<template>
  <div :status="status" class="column">
    <header>
      <h3>{{ name }}</h3>
    </header>
    <draggable @end="onEnd" @move="onMove" group="cards" v-model="columnCards">
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
        const cardIndex = evt.oldIndex;
        const oldStatus = evt.item.getAttributeNode("status").value;
        const newStatus = evt.to.parentNode.getAttributeNode("status").value;
        console.log(cardIndex);
        console.log(oldStatus);
        console.log(newStatus);
        this.$emit('update-card', cardIndex, oldStatus, newStatus);
      },
      onMove: function () {
        return false;
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
