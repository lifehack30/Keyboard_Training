<template>
  <div class="keyboard">
    <div class="row">
      <div
        v-for="key in row1"
        :key="key"
        class="key"
        :class="{ active: pressedKeys.includes(key) }"
      >
        {{ key }}
      </div>
    </div>
    <div class="row">
      <div
        v-for="key in row2"
        :key="key"
        class="key"
        :class="{ active: pressedKeys.includes(key) }"
      >
        {{ key }}
      </div>
    </div>
    <div class="row">
      <div
        v-for="key in row3"
        :key="key"
        class="key"
        :class="{ active: pressedKeys.includes(key) }"
      >
        {{ key }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      row1: ['Й', 'Ц', 'У', 'К', 'Е', 'Н', 'Г', 'Ш', 'Щ', 'З', 'Х', 'Ъ'],
      row2: ['Ф', 'Ы', 'В', 'А', 'П', 'Р', 'О', 'Л', 'Д', 'Ж', 'Э'],
      row3: ['Я', 'Ч', 'С', 'М', 'И', 'Т', 'Ь', 'Б', 'Ю', '|__|'],
      pressedKeys: [] // Массив для хранения нажатых клавиш
    }
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeyDown)
    window.addEventListener('keyup', this.handleKeyUp)
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeyDown)
    window.removeEventListener('keyup', this.handleKeyUp)
  },
  methods: {
    handleKeyDown(event) {
      const key = event.key === ' ' ? ' ' : event.key.toUpperCase()
      if (!this.pressedKeys.includes(key)) {
        this.pressedKeys.push(key)
      }
    },
    handleKeyUp(event) {
      const key = event.key === ' ' ? ' ' : event.key.toUpperCase()
      const index = this.pressedKeys.indexOf(key)
      if (index > -1) {
        this.pressedKeys.splice(index, 1)
      }
    }
  }
}
</script>

<style scoped>
.keyboard {
  display: flex;
  flex-direction: column;
  max-width: 1200px;
  margin: 20px auto;
  border: 1px solid #ccc;
  padding: 10px;
  background-color: #181818;
}

.row {
  display: flex;
  justify-content: center;
  margin: 5px 0;
}

.key {
  width: 70px;
  height: 60px;
  margin: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #e0e0e0;
  border-radius: 5px;
  transition: background-color 0.2s;
  font-size: 20px;
}

.key.active {
  background-color: #581c87;
}
</style>
