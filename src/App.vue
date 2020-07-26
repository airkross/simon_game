<template>
  <div>
    <h1>Simon Says by Ruslan Guseinov</h1>
    <div id="app">
      <!-- сам кружок с кнопками -->
      <div class="circle" ref="block">
        <div class="block-1 block" @click="start?play('1'):false"></div>
        <div class="block-2 block" @click="start?play('2'):false "></div>
        <div class="block-3 block" @click="start?play('3'):false "></div>
        <div class="block-4 block" @click="start?play('4'):false "></div>
      </div>
      <!-- панель настроек -->
      <div class="right">
        <div class="rounds"></div>
        <button class="btn-start" @click="startGame()">Start</button>
        <h3>Раунд: {{rounds}}</h3>
        <h3>Режим игры</h3>
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
  </div>
</template>

<script>
// имортирует треки
import audio1 from "./assets/1.mp3";
import audio2 from "./assets/2.mp3";
import audio3 from "./assets/3.mp3";
import audio4 from "./assets/4.mp3";
export default {
  name: "App",
  data() {
    return {
      options: "1500", //свойство для определения длины раунда
      optionMode: "1", //свойство для определения режима игры
      start: false, //флаг проверки начала игры, пока false - кнопки в кружке не активны
      steps: [], //массиф ходов, сюда пушить будем новые ходы
      counterPick: 0, //каунтер, который итерирует элементы в массиве steps когда мы нажмаем по кнопка в кружке
      rounds: 1, //каунтер раундов
      /*
       * флаг, который делаем малозаметную фичу, а именно, когда начинается новый раунд и программа нам
       * наигрывает мелодию для того чтобы мы запомнили, в этот же момент,
       * данный флаг делает кнопки в кружке неактивным (заметил что референс сайте также)
       * если попытаться нажать, то в консоль вылетет сообщение пасхалка
       */
      playSound: false, //
    };
  },
  methods: {
    // метод начала игры
    startGame() {
      this.steps = [];
      this.counterPick = 0;
      this.start = true;
      this.rounds = 1;
      if (this.optionMode == 4) {
        this.freeBoard();
      } else {
        this.startFoo();
      }
    },
    // отдельный метод, который разрешает играться с кружочком как душе угодно
    freeBoard(ind) {
      let audio = new Audio();
      if (ind == 1) {
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
      } else if (ind == 2) {
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
      } else if (ind == 3) {
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
      } else if (ind == 4) {
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
      }
      audio.play();
    },
    //  метод, который пушит новые шаги и самостоятельно вопспроизводит последовательность
    startFoo() {
      this.playSound = true;
      this.steps.push(Math.floor(Math.random() * 4 + 1));
      let counter = 0;
      if (this.optionMode == 1) {
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
        }, Number.parseInt(this.options));
        counter = 0;
      } else if (this.optionMode == 2) {
        let audio = new Audio();
        let interval = setInterval(() => {
          if (this.steps[counter] == 1) {
            audio.src = audio1;
          } else if (this.steps[counter] == 2) {
            audio.src = audio2;
          } else if (this.steps[counter] == 3) {
            audio.src = audio3;
          } else {
            audio.src = audio4;
          }
          counter++;
          audio.play();
          if (counter == this.steps.length) {
            this.playSound = false;
            clearInterval(interval);
          }
        }, Number.parseInt(this.options));
        counter = 0;
      } else if (this.optionMode == 3) {
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
          }
          counter++;
          if (counter == this.steps.length) {
            this.playSound = false;
            clearInterval(interval);
          }
        }, Number.parseInt(this.options));
        counter = 0;
      }
    },
    // обработчик, который реализует весь функционал игры связанный с непосредственными нажатиями на кнопки внутри кружочка Пользователем
    play(ind) {
      if (this.optionMode == 4) {
        this.freeBoard(ind);
      }
      if (this.playSound) {
        console.log("Эй ,куда спешишь, Сталкер,брат,погоди!");
        return;
      }
      let audio = new Audio();
      if (this.optionMode == 1) {
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
            alert(
              "Привет, ты был так близко, ты просто взгляни, вот массив ходов: [" +
                this.steps +
                "]\n!Количество пройденных раундов:" +
                (this.rounds - 1)
            );
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
            alert(
              "Ты хорошо сражался, Воин!\nКоличество пройденных раундов:" +
                (this.rounds - 1)
            );
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
            alert(
              "Мой взор застилает тьма... © Артас\nКоличество пройденных раундов:" +
                (this.rounds - 1)
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
            alert(
              "Ты это... заходи как-нибудь еще...\nКоличество пройденных раундов:" +
                (this.rounds - 1)
            );
            this.rounds = 1;
          }
        }
      } else if (this.optionMode == 2) {
        if (ind == "1") {
          if (this.steps[this.counterPick] == ind) {
            audio.src = audio1;
          } else {
            this.start = false;
            alert(
              "Между первой и второй, перерывчик не большой... Первой и второй игрой конечно же :)\n!Количество пройденных раундов:" +
                (this.rounds - 1)
            );
            this.rounds = 1;
          }
        } else if (ind == "2") {
          if (this.steps[this.counterPick] == ind) {
            audio.src = audio2;
          } else {
            this.start = false;
            alert(
              "ПОТРАЧЕНО!\nКоличество пройденных раундов:" + (this.rounds - 1)
            );
            this.rounds = 1;
          }
        } else if (ind == "3") {
          if (this.steps[this.counterPick] == ind) {
            audio.src = audio3;
          } else {
            this.start = false;
            alert(
              "Ого... Таких рекордов даже я не ставил...\nКоличество пройденных раундов:" +
                (this.rounds - 1)
            );
            this.rounds = 1;
          }
        } else {
          if (this.steps[this.counterPick] == ind) {
            audio.src = audio4;
          } else {
            this.start = false;
            alert(
              "Это была хорошая игра, давай как-нибудь повторим!\nКоличество пройденных раундов:" +
                (this.rounds - 1)
            );
            this.rounds = 1;
          }
        }
      } else if (this.optionMode == 3) {
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
          } else {
            this.start = false;
            alert(
              "В следующий раз у тебя обязательно получится!\nКоличество пройденных раундов:" +
                (this.rounds - 1)
            );
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
          } else {
            this.start = false;
            alert(
              "Ну как же так, ты был так близко :(\n!Количество пройденных раундов:" +
                (this.rounds - 1)
            );
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
          } else {
            this.start = false;
            alert(
              "Попробуем снова?\nКоличество пройденных раундов:" +
                (this.rounds - 1)
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
          } else {
            this.start = false;
            alert(
              "Ты проиграл, но не пичалься :)\nКоличество пройденных раундов:" +
                (this.rounds - 1)
            );
            this.rounds = 1;
          }
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
h1 {
  text-align: center;
}
</style>
