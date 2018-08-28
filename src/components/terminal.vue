<template>
  <div id="wrapper" 
    :class="{ active: isActive, textColor: colorActive }" >
    <header>
        <h1 class="title">{{title}}</h1>
        <div class="buttons">
            <button @click="darkTheme()" v-show="isActive">Dark theme</button>
            <button @click="lightTheme()" v-show="!isActive">Light theme</button>
        </div>
    </header>
    <div id="cmd" ref="cli">
        Commands you can use:
        <ul type="square">
            <li v-for="(item, index) in info" :key='index'>
                {{ item }}
            </li>
        </ul>
        <div>
            This terminal is fully interactive and also
            dynamic for some commands.
        </div><br />
        <div>
        <input 
        class="gameInput"
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
      info: [
        "Date",
        "Time",
        "Info",
        "UserAgent",
        "ColorRed",
        "ColorGreen",
        "GuessGame",
        "Clear",
        "About"
      ],
      title: "Terminal",
      isActive: false,
      isGameActive: false,
      colorActive: false,
      text: "",
      msg: "",
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
      this.msg = this.text;
      this.checkInput(this.text);
      this.text = "";
      this.randomNum = this.genereteRandomNum();
      e.preventDefault();
    },

    genereteRandomNum(){
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
        this.arr.push("Correct number!");
        this.arr.push("---------------");
        this.arr.push("Want to play Again?");
        this.arr.push("Type GuessGame");
      }
    },

    guessNumber() {
      this.isGameActive = true;
      this.arr = [];
      this.arr.push("Guess a computer chosen number from 1-100");
    },

    getNewDate() {
      this.msg = new Date();
      this.arr.push(this.msg);
    },

    getTimer() {
      this.msg = new Date().getHours() + ":" + new Date().getMinutes();
      this.arr.push(this.msg);
    },

    getInfo() {
      this.msg = "Commands you can use are: " + this.info.join(", ");
      this.arr.push(this.msg);
    },

    clearData() {
      this.arr = [];
      this.isGameActive = false;
    },

    getUserAgent() {
      this.msg = navigator.userAgent;
      this.arr.push(this.msg);
    },

    colorRed() {
      this.colorActive = true;
    },

    colorGreen() {
      this.colorActive = false;
    },

    about() {
      this.arr.push(
        "Created by Jivko Jelev & Svetozar Iliev",
        "Created on 28 Aug 2018",
        "Technology: HTML, CSS, JS",
        "Frameworks: Vue.js"
      );
    },

    unknownWords() {
      this.msg = "Command not found!";
      this.arr.push(this.msg);
    },

    checkInput(txt) {
      const check =
        txt == "Date" ||
        txt == "Time" ||
        txt == "Info" ||
        txt == "Clear" ||
        txt == "About" ||
        txt == "ColorRed" ||
        txt == "UserAgent" ||
        txt == "ColorGreen" ||
        txt == "GuessGame";

      if (check) {
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

      return this.unknownWords();
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

.gameInput {
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
</style>