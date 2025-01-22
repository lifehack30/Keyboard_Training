<script>
import MainPage from './components/MainPage.vue';
import Game from './components/Game.vue'; // Импортируем компонент Game



export default {
  name: 'App',
  components: {
    MainPage,
    Game,


  },
  data() {
    return {
      currentComponent: 'MainPage'
    };
  },
  methods: {
    goToGame() {
      this.currentComponent = 'Game'; // Переключаем на компонент Game
    },
  goToMainPage() {
      this.currentComponent = 'MainPage'; // Переключаем на компонент MainPage
    }
}}
</script>

<template>
  <header class="border-solid border-2 border-black pl-2 background2">
    <div class="grid grid-cols-3 gap-4">
      <img class="w-12 h-12" src="/KeyBoard.svg" alt="cart" />
      <h1 class="text-center text-white p-3">Клавиатурный тренажёр</h1>
      <button
        v-if="currentComponent === 'MainPage'"
        @click="goToGame"
        class="p-2 border-solid border-4 border-purple-700 rounded-md duration-300 transform hover:shadow-lg background-purple-700 text-white bg-purple-700 hover:bg-purple-900 hover:border-purple-900"
      >
        Сыграть в игру
      </button>
      <button
        v-if="currentComponent === 'Game'"
        @click="goToMainPage"
        class="p-2 border-solid border-4 border-green-700 rounded-md duration-300 transform hover:shadow-lg background-green-700 text-white bg-green-700 hover:bg-green-900 hover:border-green-900"
      >
        Вернуться
      </button>
    </div>
  </header>
  <component :is="currentComponent"></component>
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
