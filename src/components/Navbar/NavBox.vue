<template>
  <div class="navbox">
    <h3 class="navBoxTitle">
      {{ data.header }}
    </h3>
    <p
      v-for="(item, index) in data.list"
      :key="index"
      :class="item.label == 'Inbox' || item.label == 'Contacts' ? 'active' : ''"
    >
      <i :class="item.icon"></i> {{ item.label }}
    </p>
  </div>
</template>
<script lang="ts">
import { defineComponent, type PropType } from 'vue'

export default defineComponent({
  name: 'NavBox',
  props: {
    data: {
      type: Object as PropType<{ header: string; list: { icon: string; label: string }[] }>,
      required: true
    }
  }
})
</script>
<!-- Aktív ellenrőzést igazából routeon kéne csinálni, és ott ha egyenlő akkor szimplán egy class-al rárakni csak az activeot és akkor működik is az active -->
<!-- ez miatt itt most csak labelre ellenőrzök de amúgy ez nem helyes így teljes mértékben(label-ra nem szokásom ellenőrizni) -->
<style lang="scss">
.navbox {
  height: auto;
  width: 90%;
  p,
  h3 {
    margin: 1.5vh;
  }
  h3 {
    text-transform: uppercase;
    font-size: 1.2vh;
    font-weight: 700;
    color: black;
  }
  p {
    font-size: 1.7vh;
    font-weight: 500;
    width: 100%;
    position: relative;
    color: rgb(102, 102, 102);
    i {
      margin-right: 1vh;
    }
    &:hover {
      cursor: pointer;
      font-weight: 700;
    }
  }
  .active {
    color: #4E37D3;
    font-weight: 700;
    &:after {
      content: ' ';
      position: absolute;
      right: -0.5vh;
      transform: rotate(45deg);
      top: 0.1vh;
      display: block;
      height: 1.5vh;
      width: 1.5vh;
      background-color: white;
    }
  }
}
</style>
