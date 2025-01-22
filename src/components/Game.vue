<template>
  <div class="background2 fullscreen-div trainer">
    <div v-if="!isGameStarted" class="container py-10 px-10 mx-0 min-w-full flex flex-col items-center">
      <button  @click="startGame" class="p-2 border-solid border-4 border-purple-700 rounded-md duration-300 transform hover:shadow-lg background-purple-700 text-white bg-purple-700 hover:bg-purple-900 hover:border-purple-900">Начать игру</button>
    </div>
    <div v-else class="game-container">
      <div class="player">
        <div class="word-to-type">
          <span
            v-for="(letter, index) in currentWord"
            :key="index"
            :class="{
              correct: typedWord[index] === letter,
              incorrect: typedWord[index] !== letter && typedWord[index]
            }"
          >
            {{ letter }}
          </span>
        </div>
        <input
          type="text"
          v-model="typedWord"
          @input="checkWord"
          placeholder="Начни писать мой друг..."
        />
      </div>

      <div class="game-info">
        <div class="score">Очки: {{ score }}</div>
        <div class="lives">
          <svg
            v-for="n in lives"
            :key="n"
            class="heart"
            viewBox="0 0 24 24"
          >
            <path
              d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
            />
          </svg>
        </div>
        <div class="timer">Времени осталось: {{ timer }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isGameStarted: false,
      words: [
        "attack", "defend", "enemy", "victory", "hero", "battle", "warrior", "sword",
        "shield", "castle", "dragon", "magic", "knight", "adventure", "quest", "kingdom",
        "archer", "treasure", "explore", "goblin", "orc", "wizard", "dungeon", "power",
        "strength", "challenge", "defeat", "strike", "champion", "bravery", "darkness",
        "light", "journey", "danger", "fortress", "elixir", "courage", "legend", "mystic",
        "sorcery", "guardian", "royalty", "archmage", "stealth", "spell", "trap", "phantom",
        "riddle", "rogue", "bounty", "potion", "healer", "warlock", "fortify", "strategy",
        "ally", "rescue", "victorious", "fearless", "clash", "realm", "fate", "destiny",
        "hunter", "relic", "artifact", "summon", "banish", "vanquish", "conquer", "siege",
        "brutal", "triumph", "glory", "epic", "arena", "villain", "sinister", "noble",
        "betrayal", "loyalty", "honor", "justice", "revenge", "ambush", "mercenary",
        "guild", "faction", "elite", "prowess", "ascend", "mighty", "vengeance", "wrath"
      ],
      currentWord: "",
      typedWord: "",
      score: 0,
      lives: 3,
      timer: 60,
      timerInterval: null,
    };
  },
  methods: {
    startGame() {
      this.isGameStarted = true;
      this.resetGame();
    },
    setNewWord() {
      this.currentWord = this.words[Math.floor(Math.random() * this.words.length)];
      this.typedWord = "";
    },
    checkWord() {
      if (this.typedWord === this.currentWord) {
        this.score += 1;
        this.setNewWord();
      } else if (!this.currentWord.startsWith(this.typedWord)) {
        this.lives -= 1;
        this.typedWord = "";

        if (this.lives === 0) {
          alert("Игра окончена! Твой счет: " + this.score);
          this.endGame();
        }
      }
    },
    startTimer() {
      if (this.timerInterval) {
        clearInterval(this.timerInterval);
      }
      this.timerInterval = setInterval(() => {
        this.timer -= 1;

        if (this.timer <= 0) {
          clearInterval(this.timerInterval);
          alert("Время вышло! Твой счет: " + this.score);
          this.endGame();
        }
      }, 1000);
    },
    resetGame() {
      this.lives = 3;
      this.score = 0;
      this.timer = 60;
      this.setNewWord();
      this.startTimer();
    },
    endGame() {
      clearInterval(this.timerInterval);
      this.isGameStarted = false;
    },
  },
  beforeDestroy() {
    clearInterval(this.timerInterval);
  },
};
</script>
  
  <style scoped>
  .background2 {
  background-color: #181818; /* Замените на желаемый цвет */
  /* Например, на всю высоту окна */
}
  .game-container {
    position: relative;
    width: 100%;
    max-width: 800px;
    height: 600px;
    margin: 20px auto; /* Центрируем игру под шапкой */
    border: 2px solid #333;
    background-color: #fff;
    overflow: hidden;
  }
  
  .player {
    position: absolute;
    bottom: 50px;
    left: 50%;
    transform: translateX(-50%);
    text-align: center;
  }
  
  .word-to-type {
    margin-top: 10px;
    font-size: 20px;
    font-weight: bold;
  }
  
  .word-to-type span {
    padding: 0 2px;
  }
  
  .word-to-type .correct {
    color: green;
  }
  
  .word-to-type .incorrect {
    color: red;
  }
  
  .player input {
    margin-top: 10px;
    padding: 10px;
    font-size: 16px;
  }
  
  .game-info {
    position: absolute;
    top: 10px;
    left: 10px;
  }
  .fullscreen-div {
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
}
  .score,
  .lives,
  .timer {
    margin-bottom: 10px;
    font-size: 18px;
  }
  
  .heart {
    width: 24px;
    height: 24px;
    fill: red;
  }
  .trainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}
  </style>
  