<template>
  <div class="board-container">
    <div class="board">
      <Column :cards="todoCards" :key="1" @update-card="updateCard" name="TO DO" status="TODO">
      </Column>
      <Column :cards="doingCards" :key="2" @update-card="updateCard" name="DOING" status="DOING">
      </Column>
      <Column :cards="doneCards" :key="3" @update-card="updateCard" name="DONE" status="DONE">
      </Column>
    </div>
  </div>

</template>

<script>
  import Column from './Column.vue'

  export default {
    name: 'Board',
    components: {Column},
    props: {
      msg: String,
    },
    computed: {
      todoCards: function () {
        return this.cards.filter(card => card.status === 'TODO');
      },
      doingCards: function () {
        return this.cards.filter(card => card.status === 'DOING');
      },
      doneCards: function () {
        return this.cards.filter(card => card.status === 'DONE');
      },
    },
    data: function () {
      return {
        cards: [],
      }
    },
    mounted: function () {
      // Fetch data
      this.axios
              .get('http://localhost:8081/api/card')
              .then(response => {
                this.cards = response.data
              })
              .catch(error => {
                console.error('Error fetch cards: ' + error.message);
              })
    },
    methods: {
      updateCard: function (card, newStatus) {
        const oldStatus = card.status;
        card.status = newStatus;
        this.axios
                .put('http://localhost:8081/api/card/' + card.id, card)
                .then(response => {
                  console.log('Successfully update card: ' + JSON.stringify(response.status));
                })
                .catch(error => {
                  // In case of error, revert drag & drop
                  card.status = oldStatus;
                  console.log('Error update card: ' + error.message);
                });
      },
    }
  }
</script>

<style scoped>
  .board {
    display: flex;
  }
</style>
