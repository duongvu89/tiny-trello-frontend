<template>
  <div :status="status" class="column">
    <header>
      <h3>{{ name }}</h3>
    </header>
    <draggable group="cards" v-model="columnCards" @end="onDragEnd">
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
      name: null,
      cards: null,
      status: null,
    },
    // Works with watch
    watch: {
      cards: function (newCards) {
        this.columnCards = newCards;
      }
    },
    data: function () {
      return {
        columnCards: null
      }
    },
    methods: {
      onDragEnd: function (evt) {
        const draggedCard = this.cards[evt.oldIndex];
        const newStatus = evt.to.parentNode.getAttributeNode("status").value;
        if (draggedCard.status === newStatus) {
          // Prevent sorting
          this.columnCards = this.cards;
        } else {
          // Emit db update
          this.$emit('update-card', draggedCard, newStatus);
        }

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
