<template>
  <div class="board-container">
    <div class="board">
      <Column :cards="todoCards" :key="1" name="TO DO" status="TODO">
      </Column>
      <Column :cards="doingCards" :key="2" name="DOING" status="DOING">
      </Column>
      <Column :cards="doneCards" :key="3" name="DONE" status="DONE">
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
        if (!this.cards) return [];
        return this.cards.filter(card => card.status === 'TODO');
      },
      doingCards: function () {
        if (!this.cards) return [];
        return this.cards.filter(card => card.status === 'DOING');
      },
      doneCards: function () {
        if (!this.cards) return [];
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
