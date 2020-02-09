<template>
  <section>
    <div class="button-create">
      <div class="board-header">
        <div>
          <p v-if="showUpdateError" class="error-message">Error updating cards!</p>
          <p v-else-if="showFetchError" class="error-message">Error fetching cards!</p>
        </div>
        <button id="show-modal" @click="showModal = true">
          Create
        </button>
      </div>
    </div>
    <card-modal v-if="showModal" @save="createCard" @close="showModal = false" :show-error="showCreateError">
    </card-modal>
    <div class="board">
      <column :cards="todoCards" :key="1" @update-card="updateCard" name="To Do" status="TODO">
      </column>
      <column :cards="doingCards" :key="2" @update-card="updateCard" name="Doing" status="DOING">
      </column>
      <column :cards="doneCards" :key="3" @update-card="updateCard" name="Done" status="DONE">
      </column>
    </div>
  </section>
</template>

<script>
  import Column from './Column.vue'
  import CardModal from "./CardModal.vue"

  export default {
    name: 'Board',
    components: { Column, CardModal },
    data: function () {
      return {
        cards: [],
        showModal: false,
        showCreateError: false,
        showFetchError: false,
        showUpdateError: false
      }
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
    mounted: function () {
      // Fetch data
      this.axios
              .get('http://localhost:8081/api/card')
              .then(response => {
                this.cards = response.data
              })
              .catch(error => {
                console.error('Error fetching cards: ' + error.message);
                this.showFetchError = true;
              })
    },
    methods: {
      createCard: function (description) {
        if (!description || description.trim() === '') return;
        let card = {description: description.trim(), status: 'TODO'};
        this.axios
                .post('http://localhost:8081/api/card', card)
                .then(response => {
                  this.showModal = false;
                  this.showCreateError = false;
                  console.log('Successfully create card.');
                  card.id = response.data;
                  this.cards.push(card);
                })
                .catch(error => {
                  console.log('Error creating card: ' + error.message);
                  this.showCreateError = true;
                });
      },
      updateCard: function (card, newStatus) {
        const oldStatus = card.status;
        card.status = newStatus;
        this.axios
                .put('http://localhost:8081/api/card/' + card.id, card)
                .then(response => {
                  console.log('Successfully update card: ' + response.status);
                })
                .catch(error => {
                  // In case of error, revert drag & drop
                  card.status = oldStatus;
                  this.showUpdateError = true;
                  console.log('Error updating card: ' + error.message);
                });
      },
    }
  }
</script>

<style scoped>
  .board-header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
  .error-message {
    color: red;
    font-size: 13px;
  }

  .board {
    display: flex;
    flex-direction: row;
  }

  .button-create {
    text-align: right;
  }
  .button-create button {
    background-color: #42b983;
    border: none;
    color: white;
    padding: 7px 12px;
    font-size: 15px;
    margin: 4px 2px;
  }
</style>
