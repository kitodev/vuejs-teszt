<template>
  <div class="modal">
    <div class="modal-content">
      <span class="close" @click="closeModal">X</span>
      <p>
        Do you want to delete <br /><br /><strong>{{ text }}</strong> from the List?
      </p>
      <div class="buttons">
        <button class="delete-button" @click="deleteUsers">Delete</button>
        <button class="cancel-button" @click="closeModal">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PopUp',
  props: {
    text: {
      type: String,
      default: ''
    }
  },
  methods: {
    closeModal() {
      this.$emit('close')
    },
    deleteUsers() {
      this.$emit('delete')
    }
  }
}
</script>

<style lang="scss" scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 9999;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);

  .modal-content {
    background-color: #fefefe;
    margin: auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    max-width: 400px;
    text-align: center;
    border-radius: 10px;
    position: relative;

    .close {
      color: #aaa;
      float: right;
      font-size: 2vh;
      font-weight: bold;
      cursor: pointer;
    }

    .close:hover,
    .close:focus {
      color: black;
    }

    p {
      font-size: 2vh;
      margin-bottom: 4vh;
    }

    .buttons {
      display: flex;
      justify-content: center;
      margin-top: 20px;

      button {
        height: 4vh;
        width: 13vh;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-size: 16px;
        margin: 0 10px;
      }

      .delete-button {
        background-color: #475de5;
        color: white;
      }

      .cancel-button {
        background-color: #f1f1f1;
        color: #000;
      }
    }
  }
}
</style>
