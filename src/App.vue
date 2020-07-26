<template>
  <div id="app">
    <div class="circle" ref="block">
      <div class="block-1 block" @click="start?play('1'):false"></div>
      <div class="block-2 block" @click="start?play('2'):false "></div>
      <div class="block-3 block" @click="start?play('3'):false "></div>
      <div class="block-4 block" @click="start?play('4'):false "></div>
    </div>
    <div class="right">
      <div class="rounds"></div>
      <button class="btn-start" @click="startGame()">Start</button>
      <h3>Round: {{rounds}}</h3>
      <h3>Game Options</h3>
      <div class="radio-wrap">
        <input type="radio" v-model="optionMode" value="1" id="normal" />
        <label for="normal">Normal</label>
      </div>
      <div class="radio-wrap">
        <input type="radio" v-model="optionMode" value="2" id="sound" />
        <label for="sound">Sound Only</label>
      </div>
      <div class="radio-wrap">
        <input type="radio" v-model="optionMode" value="3" id="light" />
        <label for="light">Light only</label>
      </div>
      <div class="radio-wrap">
        <input type="radio" v-model="optionMode" value="4" id="free" />
        <label for="free">Free board</label>
      </div>
      <pre>-------------------</pre>
      <h3>Уровни из ТЗ</h3>
      <div class="radio-wrap">
        <input type="radio" selected v-model="options" value="1500" id="easy" />
        <label for="easy">Easy lvl 1.5 sec</label>
      </div>
      <div class="radio-wrap">
        <input type="radio" v-model="options" value="1000" id="normal1" />
        <label for="normal1">Normal lvl 1.0 sec</label>
      </div>
      <div class="radio-wrap">
        <input type="radio" v-model="options" value="400" id="hard1" />
        <label for="hard1">Hard lvl 0.4 sec</label>
      </div>
    </div>
  </div>
</template>

<script>
import audio1 from "./assets/1.mp3";
import audio2 from "./assets/2.mp3";
import audio3 from "./assets/3.mp3";
import audio4 from "./assets/4.mp3";
export default {
  name: "App",
  data() {
    return {
      options: "1500",
      optionMode: '1',
      start: false,
      steps: [],
      counterPick: 0,
      rounds: 1,
      playSound: false,
    };
  },
  watch: {
    options: function (val) {
      console.log(val);
    },
  },
  methods: {
    startGame() {
      this.steps = [];
      this.counterPick = 0;
      this.start = true;
      this.rounds = 1;
      this.startFoo();
    },
    startFoo() {
      this.playSound = true;
      this.steps.push(Math.floor(Math.random() * 4 + 1));
      let counter = 0;
      // if(this.optionMode == 1){}
      let audio = new Audio();
      let interval = setInterval(() => {
        if (this.steps[counter] == 1) {
          this.$refs.block.getElementsByClassName("block")[
            this.steps[counter] - 1
          ].style.background = "red";
          let step = this.steps[counter] - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#f85656";
          }, 250);
          audio.src = audio1;
        } else if (this.steps[counter] == 2) {
          this.$refs.block.getElementsByClassName("block")[
            this.steps[counter] - 1
          ].style.background = "green";
          let step = this.steps[counter] - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#4da14d";
          }, 250);
          audio.src = audio2;
        } else if (this.steps[counter] == 3) {
          this.$refs.block.getElementsByClassName("block")[
            this.steps[counter] - 1
          ].style.background = "blue";
          let step = this.steps[counter] - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#6161ff";
          }, 250);
          audio.src = audio3;
        } else {
          this.$refs.block.getElementsByClassName("block")[
            this.steps[counter] - 1
          ].style.background = "yellow";
          let step = this.steps[counter] - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#f5f578";
          }, 250);
          audio.src = audio4;
        }
        counter++;
        audio.play();
        if (counter == this.steps.length) {
          this.playSound = false;
          clearInterval(interval);
        }
        console.log(this.playSound);
      }, Number.parseInt(this.options));
      counter = 0;
    },
    play(ind) {
      if (this.playSound) {
        console.log("Hello STOP!!!");
        return;
      }
      let audio = new Audio();
      if (ind == "1") {
        if (this.steps[this.counterPick] == ind) {
          this.$refs.block.getElementsByClassName("block")[
            ind - 1
          ].style.background = "red";
          let step = ind - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#f85656";
          }, 250);
          audio.src = audio1;
        } else {
          this.start = false;
          console.log(ind);
          console.log(this.steps[this.counterPick]);
          alert("Вы проиграли!Количество пройденных раундов:" + this.rounds - 1);
          this.rounds = 1;
        }
      } else if (ind == "2") {
        if (this.steps[this.counterPick] == ind) {
          this.$refs.block.getElementsByClassName("block")[
            ind - 1
          ].style.background = "green";
          let step = ind - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#4da14d";
          }, 250);
          audio.src = audio2;
        } else {
          this.start = false;
          console.log(ind);
          console.log(this.steps[this.counterPick]);
          alert("Вы проиграли!Количество пройденных раундов:" + this.rounds - 1);
          this.rounds = 1;
        }
      } else if (ind == "3") {
        if (this.steps[this.counterPick] == ind) {
          this.$refs.block.getElementsByClassName("block")[
            ind - 1
          ].style.background = "blue";
          let step = ind - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#6161ff";
          }, 250);
          audio.src = audio3;
        } else {
          this.start = false;
          console.log(ind);
          console.log(this.steps[this.counterPick]);
          alert(
            "Вы проиграли!Количество пройденных раундов:" + this.rounds - 1
          );
          this.rounds = 1;
        }
      } else {
        if (this.steps[this.counterPick] == ind) {
          this.$refs.block.getElementsByClassName("block")[
            ind - 1
          ].style.background = "yellow";
          let step = ind - 1;
          setTimeout(() => {
            this.$refs.block.getElementsByClassName("block")[
              step
            ].style.background = "#f5f578";
          }, 250);
          audio.src = audio4;
        } else {
          this.start = false;
          console.log(ind);
          console.log(this.steps[this.counterPick]);
          alert(
            "Вы проиграли!Количество пройденных раундов:" + this.rounds - 1
          );
          this.rounds = 1;
        }
      }
      if (this.counterPick == this.steps.length - 1 && this.start) {
        this.counterPick = 0;
        this.rounds++;
        this.startFoo();
      } else {
        this.counterPick++;
      }
      audio.play();
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.right {
  margin-left: 100px;
  display: flex;
  flex-direction: column;
}
.circle {
  width: 300px;
  height: 300px;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 50%;
  display: flex;
  flex-wrap: wrap;
  overflow: hidden;
  .block {
    width: 50%;
    height: 50%;
    cursor: pointer;
  }
  .block-1 {
    background: #f85656;
  }
  .block-2 {
    background: #4da14d;
  }
  .block-3 {
    background: #6161ff;
  }
  .block-4 {
    background: #f5f578;
  }
}
</style>
