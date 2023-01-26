<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
</script>

<template>
  <div class="m-20 bg-[url('./assets/images/under-construtor.jpg')]">
   
      <div class="text-3xl font-semibold m-12">PROJECT1-SEC-1-boi_046</div>
      <div class="w-full flex-col h-full justify-center">
        <img src="./assets/images/uc-icon.png" class="h-96 justify-center m-auto">
        <div class="m-auto w-96 bg-yellow-400 p-10 rounded-xl text-3xl font-extrabold text-center">Under Construction</div>
      </div>
   
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
