<template>
  <div :status="status" class="column">
    <h4>{{ name }}</h4>
    <draggable group="cards" v-model="columnCards" @end="onDragEnd">
      <card v-for="card in columnCards" :key="card.id" :card="card" :status="card.status" ></card>
    </draggable>
  </div>
</template>

<script>
  import Card from './Card.vue'
  import Draggable from "vuedraggable";

  export default {
    name: 'Column',
    components: {Card, Draggable},
    props: {
      name: String,
      cards: Array,
      status: String,
    },
    data: function () {
      return {
        columnCards: null
      }
    },
    watch: {
      cards: function (newCards) {
        this.columnCards = newCards;
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
    flex: 1;
    padding: 5px;
    min-height: 500px;
    background-color: aliceblue;
    margin: 0 2px;
    border-radius: 3px;
    font-family: sans-serif;
    font-size: 14px;
  }
  .column h4 {
    margin: 10px;
    color: #42b983;
  }
</style>
