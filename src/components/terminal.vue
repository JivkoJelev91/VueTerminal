<template>
  <div id="wrapper" 
    :class="{ active: isActive, textColor: colorActive }" >
    <header>
        <h1 class="title">Terminal</h1>
        <div class="buttons">
            <button @click="lightTheme()" v-show="isActive">Dark theme</button>
            <button @click="lightTheme()" v-show="!isActive">Light theme</button>
        </div>
    </header>
    <div id="cmd" ref="cli">
      Commands you can use:
      <ul type="square">
          <li v-for="(item, index) in commands" :key='index'>
              {{ item }}
          </li>
      </ul>
      <div>
          This terminal is fully interactive and also
          dynamic for some commands.
      </div><br />
       <div class="equals">
        =====================================================================
      </div>
      <div>
        <input 
        id="gameInput"
        :class="{ activeGame: isGameActive}"
        type="number" 
        placeholder="Enter your number..." 
        v-model="guessInput" 
        @keyup.enter.prevent="gameEnter" />
      </div>
      <div class="items" v-for="(item, index) in arr" :key='index'>{{item}}</div>
    </div>
    <div id="cmdbar">
      <input 
        type="text" 
        id="command" 
        placeholder="Enter command here..." 
        v-model="text" 
        @keyup.enter.prevent="onEnter" />
    </div>
  </div>
</template>

<script>
export default {
  name: "VueTerminal",
  data() {
    return {
      commands: [
        "Date",
        "Time",
        "Info",
        "UserAgent",
        "ColorRed",
        "ColorGreen",
        "ToggleTheme",
        "GuessGame",
        "About",
        "Clear"
      ],
      info: [
        "------------------------",
        "Commands you can use are:",
        "Data: Shows current date",
        "Time: Shows current time",
        "UserAgent: Shows current browser",
        "ColorRed: Changes the color of the text to red",
        "ColorGreen: Changes the color  of the text to green",
        "GuessGame: Starts a simple game",
        "About: Gives information about the creators of the app",
        "Clear:  Clears the console"
      ],
      aboutArr: [
        "#....#...#####...#....#...##....#...#######...#....#",
        "#....#...#...#...#....#...#.#...#......#......#....#",
        "#....#...#####...#....#...#..#..#......#......#....#",
        "#....#...#...#...#....#...#...#.#......#......#....#",
        "######...#####...######...#....##......#......######",
        "------------------------",
        "Created by Jivko Jelev & Svetozar Iliev",
        "Created on 28 Aug 2018",
        "Technology: HTML, CSS, JS",
        "Frameworks: Vue.js"
      ],
      correctNumData: [
        "Correct number!",
        "---------------",
        "Want to play Again?",
        "Type GuessGame",
        "Or type Clear"
      ],
      isActive: false,
      isGameActive: false,
      colorActive: false,
      isComplete: false,
      text: "",
      guessInput: "",
      arr: [],
      randomNum: 0,
      counter: 0
    };
  },
  methods: {
    lightTheme() {
      this.isActive = !this.isActive;
    },

    onEnter(e) {
      this.checkInput(this.text);
      this.text = "";
    },

    genereteRandomNum() {
      return Math.floor(Math.random() * 100) + 1;
    },

    gameEnter(e) {
      this.guessGame();
      this.guessInput = "";
    },

    guessGame() {
      if (this.guessInput > this.randomNum) {
        this.arr.push("Number is too Hight!");
      } else if (this.guessInput < this.randomNum) {
        this.arr.push("Number is too Low!");
      } else {
        this.arr = [];
        this.isGameActive = false;
        this.fadeIn(this.correctNumData);
      }
    },

    guessNumber() {
      this.isGameActive = true;
      this.randomNum = this.genereteRandomNum();
      this.arr = [];
      this.arr.push("Guess a computer chosen number from 1-100");
    },

    getTimer() {
      let date = new Date();
      let hours = date.getHours();
      let minutes = date.getMinutes();
      let seconds = date.getSeconds();
      let clock =
        this.appendZero(hours) +
        ":" +
        this.appendZero(minutes) +
        ":" +
        this.appendZero(seconds);
      this.arr.push(clock);
    },

    appendZero(time) {
      return time < 10 ? "0" + time : time;
    },

    clearData() {
      this.arr = [];
      this.isGameActive = false;
    },

    colorRed() {
      this.colorActive = true;
    },

    colorGreen() {
      this.colorActive = false;
    },

    getNewDate() {
      this.arr.push(new Date());
    },

    getUserAgent() {
      this.arr.push(navigator.userAgent);
    },

    getInfo() {
      if (this.isComplete || this.counter == 0) {
        this.fadeIn(this.info, this.info.length);
      }
    },

    about() {
      if (this.isComplete || this.counter == 0) {
        this.fadeIn(this.aboutArr, this.aboutArr.length);
      }
    },

    fadeIn(array, arrayLen) {
      let len = arrayLen * 500;
      this.counter++;
      this.isComplete = false;

      array.map((text, index) => {
        setTimeout(() => {
          this.arr.push(text);
        }, index * 500);
      });
      this.usePromise(len);
    },

    usePromise(len) {
      new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(this.isComplete);
        }, len);
      }).then(result => {
        this.isComplete = !result;
        this.counter = 0;
      });
    },

    unknownWords() {
      this.arr.push("Command not found!");
    },

    checkInput(txt) {
      if (!this.commands.includes(txt)) {
        return this.unknownWords();
      }
      return {
        Date: () => this.getNewDate(),
        Time: () => this.getTimer(),
        Info: () => this.getInfo(),
        Clear: () => this.clearData(),
        UserAgent: () => this.getUserAgent(),
        ColorRed: () => this.colorRed(),
        ColorGreen: () => this.colorGreen(),
        ToggleTheme: () => this.lightTheme(),
        About: () => this.about(),
        GuessGame: () => this.guessNumber()
      }[txt]();
    }
  },
  updated() {
    let elem = this.$refs.cli;
    elem.scrollTop = elem.scrollHeight - elem.clientHeight;
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#wrapper {
  color: #0e0;
  background-color: #222;
  height: 100vh;
}

#gameInput {
  display: none;
}

.active {
  background-color: #fff !important;
}

.textColor {
  color: red !important;
}

.activeGame {
  display: block !important;
  margin: 15px 0;
}

.items {
  margin: 10px 0;
}

.equals {
  word-break: break-all;
}
</style>
