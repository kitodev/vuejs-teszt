<script setup lang="ts">
import BannerComponent from '@/components/BannerComponent.vue';
import PopupComponent from '@/components/PopupComponent.vue';
import { ref } from 'vue';

const isBannerVisible = ref(false)
const isPopupOpen = ref(false)

// onMounted(() => {
//   setTimeout(() => {
//     isBannerVisible.value = !isBannerVisible.value
//   }, 10000)
// })
</script>

<template>
  <main>
    <div class="main-container">
      <Transition>
        <BannerComponent
          :openPopup="() => (isPopupOpen = true)"
          v-if="isBannerVisible"
          title="New features available!"
          description="Lorem Ipsum is simply dummy text of the printing and typesetting industry."
          buttonText="View new features"
          :onClick="() => (isPopupOpen = true)"
          :onClose="() => (isBannerVisible = false)"
        />
      </Transition>
      <Teleport to="body">
        <Transition>
          <PopupComponent
            v-if="isPopupOpen"
            inputLabel="Request new features"
            title="New Features Available"
            description='The name of your workspace will be visible to your customers. For us, it is "SAAS First". You will NOT be able to change this later.'
            closeButtonText="Cancel"
            buttonText="Send"
            :features="[
              {
                name: 'New UI/UX Designer',
                info: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry.'
              },
              {
                name: 'New FE Developer',
                info: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry.'
              }
            ]"
            :onClose="() => (isPopupOpen = false)"
            :onClick="() => (isPopupOpen = false)"
          />
        </Transition>
      </Teleport>
    </div>
  </main>
</template>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.3s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

</style>
