<template>
  <div class="navbar">
    <img :src="logo" id="logo">
    <NavBox v-for="(item, index) in navData" :key="index" :data="item" />
    <img :src="avatar" id="avatar">
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import avatar from "../../assets/avatar.png";
import logo from "../../assets/logo.svg";
import NavBox from './NavBox.vue';

export default defineComponent({
  name: "NavBar",
  components: {
    NavBox,
  },
  data() {
    return {
      logo,
      avatar,
      navData: [
        {
          header: 'Messages',
          list: [
            { icon: 'fa-solid fa-book', label: 'Inbox' },
            { icon: 'fa-solid fa-book', label: 'Outbound' },
            { icon: 'fa-solid fa-book', label: 'Contacts' },
            { icon: 'fa-solid fa-book', label: 'Statistic' }
          ]
        },
        {
          header: 'Messages',
          list: [
            { icon: 'fa-solid fa-book', label: 'Inbox' },
            { icon: 'fa-solid fa-book', label: 'Outbound' },
            { icon: 'fa-solid fa-book', label: 'Contacts' }
          ]
        },
        {
          header: 'Messages',
          list: [
            { icon: 'fa-solid fa-book', label: 'Inbox' },
            { icon: 'fa-solid fa-book', label: 'Outbound' },
            { icon: 'fa-solid fa-book', label: 'Contacts' }
          ]
        },
      ]
    };
  },
});
</script>

<style lang="scss">
.navbar {
  width: 17%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  #logo {
    width: 60%;
    height: auto;
  }
  #avatar {
    width: 30%;
    height: auto;
  }
}
</style>
