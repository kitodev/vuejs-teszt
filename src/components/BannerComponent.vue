<script setup lang="ts">
defineProps<{
  title: string
  description: string
  buttonText: string
  onClick: () => void
  onClose: () => void
}>()
</script>

<template>
  <div class="banner">
    <div class="banner__close" @click="onClose">X</div>
    <div class="banner__content">
      <div class="banner__content__item">
        <img src="/banner-icon.svg" />
      </div>
      <div class="banner__content__item">
        <h1 class="banner__title">{{ title }}</h1>
        <p class="banner__description">{{ description }}</p>
        <button class="banner__button" @click="onClick">{{ buttonText }}</button>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.banner {
  border-radius: 24px;
  background-image: radial-gradient(circle at center, #26d1b0 0%, #266ed1 100%);
  padding: 24px;
  position: relative;

  &__close {
    position: absolute;
    top: 20px;
    right: 20px;
    color: rgba(#fff, 0.5);
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
  }

  &__title {
    font-size: 48px;
    line-height: 58px;
    color: #ffffff;
    font-weight: 800;
    margin: 0 0 12px;
  }

  &__description {
    margin: 0;
    font-size: 16px;
    line-height: 18px;
    color: #e9eff2;
    font-weight: 400;
  }

  &__button {
    border: 0;
    cursor: pointer;
    border-radius: 12px;
    background-color: rgba(#fff, 0.3);
    text-align: center;
    font-size: 16px;
    line-height: 18px;
    color: #ffffff;
    font-weight: 600;
    padding: 16px 24px;
    display: inline-block;
    margin-top: 20px;
  }

  &__content {
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    gap: 60px;

    &__item {
      img {
        height: 235px;
        width: auto;
      }
    }
  }
}
</style>
