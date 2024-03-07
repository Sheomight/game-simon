<template>
  <div class="app">
    <h1>Simon game</h1>
    <div class="wrapper">
      <div class="game">
        <div class="game__field">
          <button class="game__btn" id="b1" @click="checkOrder" :disabled="!isReady"></button>
          <button class="game__btn" id="b2" @click="checkOrder" :disabled="!isReady"></button>
          <button class="game__btn" id="b3" @click="checkOrder" :disabled="!isReady"></button>
          <button class="game__btn" id="b4" @click="checkOrder" :disabled="!isReady"></button>
        </div>
        <div class="game__info">
          <h2>Раунд {{ round }}</h2>
          <div class="game__mode">
            <h3>Выберите уровень сложности</h3>
            <RadioGroup :options="gameModeOptions" :disabled="order.length > 0" v-model="gameMode"/>
          </div>
          <button @click="startRound" :disabled="order.length > 0">Начать игру</button>
        </div>
      </div>
    </div>
    <ModalWindow :show="modalVisible" @update="changeVisibility()">
      <LoseMessage />
    </ModalWindow>
  </div>
</template>

<script>
import ModalWindow from './components/ModalWindow.vue'
import LoseMessage from './components/LoseMessage.vue'
import RadioGroup from './components/RadioGroup.vue'
export default {
  name: 'App',
  components: {
    ModalWindow, 
    LoseMessage, 
    RadioGroup
  },
  data() {
    return {
      round: 1,
      order: [],
      gameMode: 1500,
      iterator: 0,
      doSound: new Audio(require('./assets/audio/do.mp3')),
      reSound: new Audio(require('./assets/audio/re.mp3')),
      miSound: new Audio(require('./assets/audio/mi.mp3')),
      faSound: new Audio(require('./assets/audio/fa.mp3')),
      isReady: false,
      modalVisible: false,
      gameModeOptions: [
        {
          label: 'Легко',
          id: 'easy',
          value: 1500,
          name: 'difficulty'
        },
        {
          label: 'Нормально',
          id: 'normal',
          value: 1000,
          name: 'difficulty'
        },
        {
          label: 'Сложно',
          id: 'hard',
          value: 400,
          name: 'difficulty'
        }
      ]
    }
  },
  methods: {
    getRandomNum() {
      return Math.floor(Math.random() * (5 - 1) + 1)
    },
    async startRound() {
      this.order.push('b' + this.getRandomNum())
      this.isReady = false
      for (let blockId of this.order) {
        await new Promise((resolve) => {
          setTimeout(() => {
            resolve('pause')
          }, this.gameMode)
        })
        await this.markBlock(blockId)
      }
      this.isReady = true
    },
    async markBlock(blockId) {
      document.getElementById(blockId).style.opacity = 1
        this.playSound(blockId)
        await new Promise((resolve) => {
          setTimeout(() => {
            resolve(document.getElementById(blockId).style.opacity = 0.6)
          }, 400)
        })
    },
    async checkOrder(e) {
      await this.markBlock(e.target.id)
      this.playSound(e.target.id)

      if (e.target.id === this.order[this.iterator]) {
        this.iterator += 1
        
        if (this.iterator === this.order.length) {
          this.round += 1
          this.iterator = 0
          this.startRound()
        }
      } else {
        this.changeVisibility()
        this.round = 1
        this.order = []
        this.isReady = false
        this.iterator = 0
      }
    },
    playSound(id) {
      switch (id) {
        case 'b1':
          this.doSound.play();
          break;
        case 'b2':
          this.reSound.play();
          break;
        case 'b3':
          this.miSound.play();
          break;
        case 'b4':
          this.faSound.play();
          break;

        default:
          break;
      }
    },
    changeVisibility() {
      this.modalVisible = !this.modalVisible
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  background-color: rgb(255, 247, 222);
}

.wrapper {
  display: flex;
}

h1 {
  text-align: center;
}

h1,
h2,
h3 {
  margin: 0;
}

.game {
  margin: 0 auto;
  display: flex;
  height: 500px;
  align-items: center;
  gap: 50px;
}

.game__field {
  width: 350px;
  height: 350px;
  background-color: gray;
  display: flex;
  flex-wrap: wrap;
  border-radius: 50%;
  overflow:hidden;
  border: 8px solid lightgray;
}

.game__btn {
  width: 50%;
  height: 50%;
  padding: 0;
  border: none;
  cursor: pointer;
  opacity: 0.6;
}

#b1 {
  background-color: rgb(255, 31, 31);
}

#b2 {
  background-color: rgb(39, 39, 255);
}

#b3 {
  background-color: rgb(40, 255, 40);
}

#b4 {
  background-color: rgb(255, 255, 46);
}

.game__info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 30px;
}
</style>
