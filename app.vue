<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>

<style>
.page-enter-from {
  transform: translateX(-100%);
}

.page-leave-to {
  transform: translateX(100%);
}

.page-enter-active,
.page-leave-active {
  transition-property: all;
  transition-timing-function: ease;
  transition-duration: 500ms;
}

.page-enter,
.page-leave-to {
  opacity: 0;
}
</style>