<script setup lang="ts">
import { ref } from 'vue'

type Feature = {
  name: string
  info: string
}

defineProps<{
  title: string
  description: string
  inputLabel: string
  closeButtonText: string
  buttonText: string
  features: Feature[]
  onClick: () => void
  onClose: () => void
}>()

const inputValue = ref<string>('')
</script>

<template>
  <div class="popup-wrapper">
    <div class="popup">
      <h2>{{ title }}</h2>
      <p>{{ description }}</p>
      <div class="popup__feature-box">
        <div v-for="feature in features" :key="feature.name" class="popup__feature">
          <span
            >{{ feature.name }} <img src="/info.svg" />
            <div class="popup__feature__info">
              <span>{{ feature.info }}</span>
            </div>
          </span>
        </div>
      </div>
      <div class="popup__input-box">
        <label for="feature-input">{{ inputLabel }}</label>
        <input type="text" id="feature-input" name="feature-input" v-model="inputValue" />
      </div>
      <div class="popup__btn-group">
        <button @click="onClose">{{ closeButtonText }}</button>
        <button class="save" @click="onClick" :disabled="!inputValue">{{ buttonText }}</button>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.popup-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 3;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  place-items: center;
  justify-content: center;
}
.popup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  min-width: 400px;
  max-width: 90%;
  background-color: white;
  border-radius: 8px;
  z-index: 4;
  border-radius: 24px;
  padding: 30px;
  filter: drop-shadow(0px 24px 14px rgba(0, 0, 0, 0.12));

  h2 {
    text-align: center;
    font-size: 25px;
    font-size: 32px;
    line-height: 28px;
    color: #1b1b1f;
    font-weight: 600;
    margin: 0 0 25px;
  }

  p {
    text-align: left;
    margin: 0 0 25px;
    font-size: 16px;
    line-height: 24px;
    color: #1b1b1f;
    font-weight: 400;
  }

  &__feature-box {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  &__feature {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    margin: 0;

    &:hover {
      .popup__feature__info {
        opacity: 1;
        visibility: visible;
      }
    }

    > span {
      position: relative;
      font-size: 16px;
      line-height: 24px;
      color: #011e1b;
      font-weight: 600;
    }

    img {
      height: 14px;
      width: auto;
    }

    &__info {
      transition: all 0.2s ease-in-out;
      box-shadow: 0px 4px 14px rgba(0, 0, 0, 0.18);
      opacity: 0;
      visibility: hidden;
      position: absolute;
      bottom: 100%;
      left: 30px;
      padding: 10px;
      border-radius: 25px;
      background-color: white;
      z-index: 5;

      &::before {
        // triangle before

        content: '';
        position: absolute;
        bottom: -20px;
        left: calc(50% - 10px);
        border-top: 10px solid white;
        border-bottom: 10px solid transparent;
        border-left: 10px solid transparent;
        border-right: 10px solid transparent;
      }
    }
  }

  &__input-box {
    margin-top: 40px;

    label {
      font-size: 16px;
      line-height: 24px;
      color: #011e1b;
      font-weight: 600;
      margin: 0 0 5px;
      display: inline-block;
    }

    input {
      all: unset;
      cursor: text;
      border-radius: 8px;
      border: 1px solid #c2ccd1;
      width: 100%;
      font-size: 16px;
      line-height: 24px;
      color: #6c7880;
      font-weight: 400;
      font-family: 'Raleway';
      padding: 4px 2px;
    }
  }

  &__btn-group {
    display: flex;
    justify-content: space-between;
    gap: 20px;
    margin-top: 30px;

    button {
      border-radius: 12px;
      background-color: #e9eff2;
      padding: 10px 20px;
      text-align: center;
      font-size: 16px;
      line-height: 18px;
      color: #1b1b1f;
      font-weight: 400;
      border: none;
      cursor: pointer;
      flex-basis: 50%;

      &.save {
        background-color: #3d22cf;
        color: white;

        &:disabled {
          background-color: lighten(#3d22cf, 20%);
          cursor: not-allowed;
        }
      }
    }
  }
}
</style>
