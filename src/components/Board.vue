<template>
  <div class="board-container">
    <button id="show-modal" @click="showModal = true">
      Create
    </button>
    <modal-new-card v-if="showModal" @save="createCard" @close="showModal = false" :show-error="showError">
      <h3 slot="header">custom header</h3>
    </modal-new-card>
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
  import ModalNewCard from "./ModalNewCard.vue"

  export default {
    name: 'Board',
    components: { Column, ModalNewCard },
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
        showModal: false,
        showError: false,
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
      createCard: function (description) {
        console.log(description);
        if (!description || description.trim() === '') return;
        let card = {description: description.trim(), status: 'TODO'};
        this.axios
                .post('http://localhost:8081/api/card', card)
                .then(response => {
                  this.showModal = false;
                  this.showError = false;
                  console.log('Successfully create card: ' + JSON.stringify(response.data));
                  card.id = response.data;
                  this.cards.push(card);
                })
                .catch(error => {
                  console.log('Error create card: ' + error.message);
                  this.showError = true;
                });
      },
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
