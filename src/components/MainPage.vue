<template>
  <div class="trainer fullscreen-div background1">
    <!-- Область вывода текста с подсветкой -->
    <div
      class="bg-white m-5 background-Input text-black w-10/12 rounded disable-select"
      style="height: auto"
    >
      <span
        class="m-1 text-3xl"
        v-for="(char, index) in displayedText"
        :key="index"
        :class="getCharClass(index)"
      >
        {{ char }}
      </span>
    </div>

    <!-- Поле для ввода текста -->
    <input
      class="rounded background-Input w-10/12 text-3xl"
      v-model="inputText"
      @input="handleInput"
      type="text"
      placeholder="Начните печатать..."
    />

    <!-- Статистика -->
    <div class="stats">
      <div class="border-solid border-2 rounded border-purple-700 text-center text-white">
        <p>Ошибок в текущем тексте: {{ errorCount }}</p>
        <p>Общее количество ошибок: {{ totalErrors }}</p>
        <p>Время: {{ timeElapsed }} сек</p>
        <p>Скорость: {{ typingSpeed }} зн/мин</p>
        <p>Время на ввод предыдущего текста: {{ lastTextTime }} сек</p>
      </div>
      <div class="text-center pt-2">
        <button
          class="border-solid border-4 border-purple-700 rounded-md duration-300 transform hover:shadow-lg background-purple-700 text-white bg-purple-700 hover:bg-purple-900 hover:border-purple-900"
          @click="resetCountError"
        >
          Сбросить количество ошибок
        </button>
      </div>
    </div>
    <TheWelcome />
  </div>
</template>

<script scoped>
import TheWelcome from './KeyBoard.vue' // Импортируем компонент клавиатуры

export default {
  name: 'TextTrainer',
  components: {
    TheWelcome // Регистрация компонента
  },
  data() {
    return {
      inputText: '',
      displayedText: '',
      errorCount: 0, // Ошибки для текущего текста
      totalErrors: 0, // Общий счётчик ошибок за сессию
      startTime: null,
      timeElapsed: 0,
      typingSpeed: 0,
      intervalId: null,
      lastTextTime: null, // Время на ввод предыдущего текста
      lastErrorCount: null, // Количество ошибок в предыдущем тексте
      textBank: [
        'Если ты в меньшинстве - и даже в единственном числе, - это не значит, что ты безумен. Есть правда и есть неправда, и, если ты держишься правды, пусть наперекор всему свету, ты не безумен.',
        'Человек, быть может, не столько ждет любви, сколько понимания.',
        'Привычка не показывать своих чувств въелась настолько, что стала инстинктом.',
        'Когда любишь кого-то, ты его любишь, и, если ничего больше не можешь ему дать, ты все-таки даешь ему любовь.',
        'Твой злейший враг, подумал он, - это твоя нервная система. В любую минуту внутреннее напряжение может отразиться на твоей наружности.',
        'Массы никогда не восстают сами по себе и никогда не восстают только потому, что они угнетены. Больше того, они даже не сознают, что угнетены, пока им не дали возможности сравнивать.',
        'В каком-то смысле книга не сообщила ему ничего нового - но в этом-то и заключалась ее прелесть. Она говорила то, что он сам бы мог сказать, если бы сумел привести в порядок отрывочные мысли. Она была произведением ума, похожего на его ум, только гораздо более сильного, более систематического и не изъязвленного страхом. Лучшие книги, понял он, говорят тебе то, что ты уже сам знаешь.',
        'Признание - не предательство. Что ты сказал или не сказал - не важно, важно только чувство. Если меня заставят разлюбить тебя - вот будет настоящее предательство.',
        'Постепенно становясь сильнейшим из побуждений, страх ломает нравственный хребет человека и заставляет его глушить в себе все чувства, кроме самосохранения.'
      ]
    }
  },
  mounted() {
    this.setRandomText()
  },
  methods: {
   
    // Установка случайного текста
    setRandomText() {
      const randomIndex = Math.floor(Math.random() * this.textBank.length)
      this.displayedText = this.textBank[randomIndex]
      this.inputText = '' // очищаем поле ввода
      this.errorCount = 0 // сбрасываем количество ошибок только для текущего текста
      this.startTime = null // сбрасываем время начала
      this.timeElapsed = 0 // сбрасываем время
      clearInterval(this.intervalId) // останавливаем таймер
    },

    resetCountError() {
      console.log('Кнопка сброса ошибок нажата')
      this.totalErrors = 0
    },
    handleInput() {
      if (this.startTime === null) {
        this.startTime = new Date()
        this.startTimer()
      }

      // Подсчет ошибок для текущего текста
      let currentErrors = 0
      for (let i = 0; i < this.inputText.length; i++) {
        if (this.inputText[i] !== this.displayedText[i]) {
          currentErrors++
        }
      }

      // Если число ошибок изменилось, добавляем новые ошибки в общий счётчик
      if (currentErrors > this.errorCount) {
        this.totalErrors += currentErrors - this.errorCount // Добавляем разницу
      }

      // Обновляем счётчик ошибок для текущего текста
      this.errorCount = currentErrors

      // Проверка на завершение ввода
      if (this.inputText === this.displayedText) {
        clearInterval(this.intervalId)

        // Сохранить время ввода текста и количество ошибок
        this.lastTextTime = this.timeElapsed
        this.lastErrorCount = this.errorCount

        // Установить новый текст
        this.setRandomText()
      }
    },
    // Подсветка символов
    getCharClass(index) {
      if (index < this.inputText.length) {
        return this.inputText[index] === this.displayedText[index] ? 'correct' : 'incorrect'
      }
      return ''
    },

    startTimer() {
      this.intervalId = setInterval(() => {
        const currentTime = new Date()
        this.timeElapsed = Math.floor((currentTime - this.startTime) / 1000)
        this.calculateSpeed()
      }, 1000)
    },
    calculateSpeed() {
      const minutesElapsed = this.timeElapsed / 60
      this.typingSpeed = Math.floor(this.inputText.length / minutesElapsed || 0)
    }
  }
  
}
</script>

<style scoped>
.textarea {
  font-size: 24px;
  font-family: Arial;
  border-radius: 8px;
  resize: none;
  padding: 5px;
  overflow: hidden;
  box-sizing: border-box;
  height: 40px;
  min-height: 40px;
  width: 400px;
  margin-bottom: 15px;
}

.trainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}

.correct {
  color: rgb(0, 255, 0);
}

.incorrect {
  color: rgb(255, 5, 5);
}

.stats {
  margin-top: 20px;
}

.background1 {
  background-color: #1f1f1f;
}

.background2 {
  background-color: #181818;
}

.fullscreen-div {
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
}

.button-color {
  background-color: #8813b1;
}

.disable-select {
  -ms-user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  user-select: none;
}
</style>
