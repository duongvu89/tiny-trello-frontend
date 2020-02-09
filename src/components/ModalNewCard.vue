<template type="text/x-template" id="modal-template">
    <transition name="modal-new-card">
        <div class="modal-mask">
            <div class="modal-wrapper">
                <div class="modal-container">
                    <div class="modal-header">
                        <h4>Summary</h4>
                    </div>
                    <div class="modal-body">
                        <textarea v-model="description" placeholder="Enter a summary for this card..." rows="4" maxlength="200"></textarea>
                    </div>

                    <div class="modal-footer">
                        <div>
                            <span v-if="showError" class="error-message">Error occur!</span>
                        </div>
                        <div class="buttons">
                            <button class="modal-default-button" @click="$emit('save', description)">
                                Save
                            </button>
                            <button class="modal-default-button" @click="$emit('close')">
                                X
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        name: 'ModalNewCard',
        props: {
            showError: String,
        },
        data: function () {
            return {
                description:null,
            }
        },
    };
</script>

<style scoped>
    .modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        display: table;
        transition: opacity .3s ease;
    }

    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
    }

    .modal-container {
        width: 300px;
        margin: 0 auto;
        padding: 20px 30px;
        background-color: aliceblue;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }

    .modal-header h4 {
        margin-top: 0;
        margin-bottom: 10px;
        color: #42b983;
        text-align: left;
    }

    .modal-body {
        margin: 10px 0;
    }

    .modal-body > textarea {
        width: 100%;
        border: none;
        outline: none;
        border-radius: 2px;
    }

    .modal-footer {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    .buttons > button {
        margin: 3px;
    }

    .error-message {
        color: red;
        font-size: 13px;
    }

    /*
     * The following styles are auto-applied to elements with
     * transition="modal" when their visibility is toggled
     * by Vue.js.
     *
     * You can easily play with the modal transition by editing
     * these styles.
     */

    .modal-enter {
        opacity: 0;
    }

    .modal-leave-active {
        opacity: 0;
    }

    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }

</style>
