<template>
    <div class="game-container">
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
          placeholder="Type the word..."
        />
      </div>
  
      <div class="game-info">
        <div class="score">Score: {{ score }}</div>
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
        <div class="timer">Time Left: {{ timer }}</div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        words: ["attack", "defend", "enemy", "victory", "hero", "battle"],
        currentWord: "",
        typedWord: "",
        score: 0,
        lives: 3,
        timer: 60,
        timerInterval: null,
      };
    },
    created() {
      this.setNewWord();
      this.startTimer();
    },
    methods: {
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
            alert("Game Over!");
            this.resetGame();
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
            alert("Time's up! Game Over!");
            this.resetGame();
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
    },
    beforeDestroy() {
      clearInterval(this.timerInterval);
    },
  };
  </script>
  
  <style scoped>
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
  </style>
  