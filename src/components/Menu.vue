<template>
    

<div id="app" class="border-solid border-2 border-black pl-2 background2">
    <router-view></router-view>
    <div class="navigation">
      <button @click="goToHome">Home</button>
      <button @click="goToGame">Game</button>
    </div>
  </div>
</template>