<template>
 <div class="layout">
  <AppHeader />
  <main class="main-content">
   <slot />
  </main>
  <AppFooter @open-contact="showContactModal = true" />
  <ContactModal v-if="showContactModal" @close="showContactModal = false" />
 </div>
</template>

<script setup>
import { ref } from "vue";

const showContactModal = ref(false);
</script>

<style lang="scss">
.layout {
 min-height: 100vh;
 display: flex;
 flex-direction: column;
}

.main-content {
 flex: 1;
}
</style>
