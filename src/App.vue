<template>
  <div id="app">

    <header>
      <h1>Саймон Говорит</h1>
    </header>

    <main class="flex-main">
      <div class="simon">
        <div class="sector s-green" id="s-green" @click="greenPress"></div>
        <div class="sector s-red" id="s-red" @click="redPress"></div>
        <div class="sector s-yellow" id="s-yellow" @click="yellowPress"></div>
        <div class="sector s-blue" id="s-blue" @click="bluePress"></div>
      </div>

      <div class="game">
        <div class="game-info">
          <span class="count">Раунд:</span>
          <span class="count" id="counter">{{turnCounter}}</span><br>
          <button class="btn-start" value="start" @click="start">Старт</button>
          <div>{{msg}}</div>
        </div>
        <div class="game-options">
          <h2>Уровень сложности:</h2>
            <input type="radio"  name="option" id="easy" @click="easy">Легкий<br>
            <input type="radio" name="option" id="normal" @click="normal" checked>Средний<br>
            <input type="radio" name="option" id="hard" @click="hard">Сложный<br>
        </div>
      </div>

    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {
},
  data() {
    return{
      order: [],
      playOrder: [],
      option: false,
      noise: true,
      timeout: 1000,
      win: false,
      flash: 0,
      intervalId: 0,
      count: 0,
      turnCounter: 0,
      good: true,
      compTurn: false,
      msg: '',

    }
  },
  methods: {
    easy() {
      let easy = document.getElementById("easy");
      if (easy.checked == true) {
        this.timeout = 1500;
        return this.timeout
      }
    },
    normal() {
      let normal = document.getElementById("normal");
      if (normal.checked == true) {
        this.timeout = 1000;
        return this.timeout
      }
    },
    hard() {
      let hard = document.getElementById("hard");
      if (hard.checked == true) {
        this.timeout = 400;
        return this.timeout
      }
    },
    start() {
      this.clearColor();
      this.msg = '';
      clearInterval(this.intervalId);
      this.play();
    },
    play() {
      this.win = false;
      this.order = [];
      this.playOrder = [];
      this.flash = 0;
      this.intervalId = 0;
      this.count = 1;
      this.turnCounter = 1;
      this.good = true;
      for (let i = 0; i < 20; i++) {
        this.order.push(Math.floor(Math.random() * 4) + 1);
      }
      this.compTurn = true;
      this.intervalId = setInterval(this.gameTurn, this.timeout)
    },
    clearColor() {
      let sGreen = document.getElementById("s-green");
      sGreen.style.backgroundColor = "green";

      let sRed = document.getElementById("s-red");
      sRed.style.backgroundColor = "darkred";

      let sYellow = document.getElementById("s-yellow");
      sYellow.style.backgroundColor = "goldenrod";

      let sBlue = document.getElementById("s-blue");
      sBlue.style.backgroundColor = "darkblue";
    },
    gameTurn() {
      if (this.flash == this.count) {
        clearInterval(this.intervalId);
        this.compTurn = false;
        this.clearColor()
      }
      if (this.compTurn) {
        this.clearColor();
        setTimeout(() => {
          if (this.order[this.flash] == 1) this.one();
          if (this.order[this.flash] == 2) this.two();
          if (this.order[this.flash] == 3) this.three();
          if (this.order[this.flash] == 4) this.four();
          this.flash++;
        }, 800)
      }
    },
    one() {
      if (this.noise) {
        let audio = document.getElementById("clip1");
        audio.play()
      }
      this.noise = true;
      let sGreen = document.getElementById("s-green");
      sGreen.style.backgroundColor = "lightgreen";
    },
    two() {
      if (this.noise) {
        let audio = document.getElementById("clip2");
        audio.play()
      }
      this.noise = true;
      let sRed = document.getElementById("s-red");
      sRed.style.backgroundColor = "red";
    },
    three() {
      if (this.noise) {
        let audio = document.getElementById("clip3");
        audio.play()
      }
      this.noise = true;
      let sYellow = document.getElementById("s-yellow");
      sYellow.style.backgroundColor = "yellow";
    },
    four() {
      if (this.noise) {
        let audio = document.getElementById("clip4");
        audio.play()
      }
      this.noise = true;
      let sBlue = document.getElementById("s-blue");
      sBlue.style.backgroundColor = "blue";
    },
    redPress() {
      this.press(2, this.two)
    },
    bluePress() {
      this.press(4, this.four)
    },
    yellowPress() {
      this.press(3, this.three)
    },
    greenPress() {
      this.press(1, this.one)
    },
    press(num, change) {
      this.playOrder.push(num);
      this.check();
      change();
      if (!this.win) {
        setTimeout(() => {
          this.clearColor();
        }, 800);
      }
    },
    check() {
      if (this.playOrder[this.playOrder.length -1] !== this.order[this.playOrder.length -1]) {
        this.good = false
      }
      if (this.playOrder.length == 20 && this.good) {
        this.winGame();
      }
      if (this.good == false) {
        this.msg = 'Вы проиграли после' + this.turnCounter + 'раундов :(';
        this.noise = false;
        this.play()
      }
      if (this.count == this.playOrder && this.good && !this.win  ) {
        this.count++;
        this.playOrder = [];
        this.compTurn = true;
        this.flash = 0;
        this.turnCounter = this.count;
        this.intervalId = setInterval(this.gameTurn, 800)
      }

    },
    winGame() {
      this.msg = 'Вы победилиБ поздравляю! Я вот ни разу не смог';
      this.win = true;
    }

  }
}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 50px auto;
  width: 600px;
}
h1{
  margin-bottom: 50px;
}
.flex-main{
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.game{
  text-align: left;
  padding-left: 50px;
}
.simon{
  display: flex;
  justify-content: space-between;
  width: 300px;
  height: 300px;
  flex-wrap: wrap;
}
  .sector{
    width: 139px;
    height: 139px;
    border: 5px solid black;
    border-radius: 15px;
  }
  .s-green{
    background-color: green;
  }
  .s-yellow{
    background-color: goldenrod;
  }
  .s-red{
    background-color: darkred;
  }
  .s-blue{
    background-color: darkblue;
  }
  .btn-start{
    border: none;
    font-size: 22px;
    border-radius: 15px;
    background-color: sandybrown;
    margin-top: 10px;
    padding: 10px 40px;
  }
  .btn-start:hover{background-color: orange}
.btn-start:active{background-color: darkorange}
  .count{
    font-size: 36px;
  }
</style>
