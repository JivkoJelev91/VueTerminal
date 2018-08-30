<template>
  <div id="wrapper" 
    :class="{ active: isActive, textColor: colorActive }" >
    <header>
        <h1 class="title">Terminal</h1>
        <div class="buttons">
            <button @click="darkTheme()" v-show="isActive">Dark theme</button>
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
        v-on:keyup.enter="gameEnter" />
      </div>
      <div class="items" v-for="(item, index) in arr" :key='index'>{{item}}</div>
    </div>
    <div id="cmdbar">
      <input 
        type="text" 
        id="command" 
        placeholder="Enter command here..." 
        v-model="text" 
        v-on:keyup.enter="onEnter" />
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
        '#....#...#####...#....#...##....#...#######...#....#',
        '#....#...#...#...#....#...#.#...#......#......#....#',
        '#....#...#####...#....#...#..#..#......#......#....#',
        '#....#...#...#...#....#...#...#.#......#......#....#',
        '######...#####...######...#....##......#......######',
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
      text: "",
      guessInput: "",
      arr: [],
      randomNum: 0
    };
  },
  methods: {
    darkTheme() {
      this.isActive = false;
    },

    lightTheme() {
      this.isActive = true;
    },

    onEnter(e) {
      this.checkInput(this.text);
      this.text = "";
      this.randomNum = this.genereteRandomNum();
      e.preventDefault();
    },

    genereteRandomNum() {
      return Math.floor(Math.random() * 100) + 1;
    },

    gameEnter(e) {
      this.guessGame();
      this.guessInput = "";
      e.preventDefault();
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
      this.arr = [];
      this.arr.push("Guess a computer chosen number from 1-100");
    },

    getNewDate() {
      this.arr.push(new Date());
    },

    getTimer() {
      let clock =
        new Date().getHours() +
        ":" +
        new Date().getMinutes() +
        ":" +
        new Date().getSeconds();
      this.arr.push(clock);
    },

    getInfo() {
      this.fadeIn(this.info);
    },

    clearData() {
      this.arr = [];
      this.isGameActive = false;
    },

    getUserAgent() {
      this.arr.push(navigator.userAgent);
    },

    colorRed() {
      this.colorActive = true;
    },

    colorGreen() {
      this.colorActive = false;
    },

    about() {
      this.fadeIn(this.aboutArr);
    },

    fadeIn(array) {
      array.map((text, index) => {
        setTimeout(() => {
          this.arr.push(text);
        }, index * 500);
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
        About: () => this.about(),
        GuessGame: () => this.guessNumber()
      }[txt]();
    }
  },
  updated() {
    var elem = this.$refs.cli;
    elem.scrollTop = elem.clientHeight;
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

.equals{
      word-break: break-all;
}
</style>
