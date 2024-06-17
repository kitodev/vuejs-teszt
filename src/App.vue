<script lang="ts">
import { defineComponent } from 'vue'
import NavBar from './components/Navbar/NavBar.vue'
import TableComponent from './components/Table/TableComponent.vue'
import HomeView from './views/HomeView.vue'

export default defineComponent({
  name: 'App',
  components: {
    NavBar,
    HomeView,
    TableComponent
  }
})
</script>

<template>
  <main class="container">
    <HomeView />
    <NavBar />
    <TableComponent />
  </main>
</template>

<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: flex;
  background-color: rgb(235, 235, 235);
}
</style>
