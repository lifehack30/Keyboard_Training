<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<template>
  <header class="border-solid border-2 border-black pl-2 background2">
    <div class="grid grid-cols-3 gap-4">
      <img class="w-12 h-12" src="/KeyBoard.svg" alt="cart" />
      <h1 class="text-center text-white p-3">Клавиатурный тренажёр</h1>
    </div>
  </header>
  <HelloWorld />
</template>

<style>
.app {
  background-color: aqua;
  background-size: cover;
  height: 100vh;
  margin: 0; /* Убедитесь, что нет отступов */
}
.background2 {
  background-color: #181818; /* Замените на желаемый цвет */
  /* Например, на всю высоту окна */
}
body {
  overflow: hidden; /* Отключает прокрутку */
}
</style>
