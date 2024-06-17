<template>
  <div class="tableRow">
    <div class="tableCell" style="width: 4%">
      <input type="checkbox" :checked="isChecked" @click="checkedHandler(user.id)" />
    </div>
    <div class="tableCell userContainer" style="width: 50%">
      <div class="avatar" :style="{ backgroundColor: getColor(userIndex + 1) }">
        {{ user.name.split(' ')[0][0] + '' + user.name.split(' ')[1][0] }}
      </div>
      <div>
        <p class="userName">{{ user.name }}</p>
        <p class="userEmail">{{ user.email }}</p>
      </div>
    </div>
    <div class="tableCell" style="width: 25%">
      <p
        class="permission"
        :style="{ background: user.permission == 'agent' ? '#C8E7F9' : '#EFE2FE' }"
      >
        {{ user.permission }}
      </p>
    </div>
    <div class="tableCell">
      <div class="actionsContainer">
        <div class="actionButton" @click="editUser">
          <img style="width: 1.8vh" :src="pencil" alt="" />
        </div>
        <div class="actionButton" @click="deleteElement">
          <img style="width: 1.8vh; height: auto" :src="bin" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import pencil from '../../assets/pencil.svg'
import bin from '../../assets/bin.svg'

export default defineComponent({
  name: 'TableUser',
  props: {
    user: {
      type: Object,
      required: true
    },
    userIndex: {
      type: Number,
      required: true
    },
    checkedUsers: {
      type: Array,
      required: true
    },
    selectedUser: {
      type: [Number, Boolean],
      default: false
    }
  },
  data() {
    return {
      pencil,
      bin
    }
  },
  computed: {
    isChecked() {
      return this.checkedUsers.includes(this.user.id)
    }
  },
  methods: {
    checkedHandler(id: number) {
      this.$emit('check', id)
    },
    editUser() {
      this.$emit('edit', this.user)
    },
    deleteElement() {
      this.$emit('showPop', this.user)
    },
    getColor(id: number) {
      if (id === 1 || id === 6) {
        return '#DE40A3'
      } else if (id % 2 === 0) {
        return '#1C40A3'
      } else {
        return '#31F1AC'
      }
    }
  }
})
</script>

<style scoped lang="scss">
input[type='checkbox'] {
  width: 24px;
  height: 24px;
  outline: none;
  border: none;
  border-radius: 8px;
}
.tableRow {
  display: flex;
  margin-bottom: 3vh;
  gap: 14px;
  border-radius: 1vh;
  padding: 1vh;
  height: 96px;
  &:hover {
    background: #f7fafc;
    border-radius: 10px;
  }
  .tableCell {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    &:first-child {
      border-top-left-radius: 1vh;
      border-bottom-left-radius: 1vh;
    }
    &:last-child {
      border-top-right-radius: 1vh;
      border-bottom-right-radius: 1vh;
    }
  }
  .userContainer {
    display: flex;
    align-items: center;
    .avatar {
      width: 58px;
      height: 58px;
      border-radius: 100%;
      display: flex;
      justify-content: center;
      font-size: 25px;
      align-items: center;
      color: white;
    }
    p {
      margin: 0.5vh 1vh;
    }
    .userName {
      font-size: 20px;
      color: black;
    }
    .userEmail {
      font-size: 16px;
      color: #bdbbbb;
    }
  }
  .permission {
    padding: 5px 10px;
    margin: 0;
    font-size: 16px;
    text-align: center;
    background: #efe2fe;
    color: #624d9c;
    border-radius: 10px;
    &::first-letter {
      text-transform: uppercase;
    }
  }
  .actionsContainer {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    margin-left: 5vh;
    gap: 1vh;
    .actionButton {
      display: none;
      justify-content: center;
      align-items: center;
      height: 4vh;
      width: 4vh;
      border: 0.1vh solid rgb(163, 163, 163);
      color: rgb(163, 163, 163);
      border-radius: 1vh;
      &:hover {
        cursor: pointer;
      }
    }
  }
  &:hover .actionsContainer .actionButton {
    display: flex;
  }
}
</style>
