<template>
  <div class="board-container">
    <div class="board">
      <Column name="TO DO" :key="1" :cards="todoCards">
      </Column>
      <Column name="DOING" :key="2" :cards="doingCards">
      </Column>
      <Column name="DONE" :key="3" :cards="doneCards">
      </Column>
    </div>


  </div>

</template>

<script>
  import Column from './Column.vue'

  export default {
    name: 'Board',
    components: { Column },
    props: {
      msg: String,
    },
    computed: {
      todoCards: function () {
        if (!this.cards) return;
        return this.cards.filter(card => card.status === 'TODO');
      },
      doingCards: function () {
        if (!this.cards) return;
        return this.cards.filter(card => card.status === 'DOING');
      },
      doneCards: function () {
        if (!this.cards) return;
        return this.cards.filter(card => card.status === 'DONE');
      },
    },
    data() {
      return {
        cards: null,
      }
    },
    mounted() {
      this.axios
              .get('http://localhost:8081/api/card')
              .then(response => {
                this.cards = response.data
              })
    }

}
</script>

<style scoped>
  .board {
    display: flex;
  }
</style>
