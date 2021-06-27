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

      <div class="game-set">
        <div class="game-info">
          <span class="count">Раунд: </span>
          <span class="count" id="counter">{{turnCounter}}</span><br>
          <button class="btn-start" value="start" @click="start">Старт</button>
          <div>{{msg}}</div>
        </div>
        <div class="game-options">
          <h2>Уровень сложности:</h2>
          <label class="option"><input type="radio"  name="option" id="easy" @click="easy">Легкий</label>
          <br>
          <label class="option"><input type="radio" name="option" id="normal" @click="normal" checked>Средний</label>
          <br>
          <label class="option"><input type="radio" name="option" id="hard" @click="hard">Сложный</label>
          <br>
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
      on: false,
      msg: '',

    }
  },
  methods: {
    easy() {
      let easy = document.getElementById("easy");
      if (easy.checked == true) {
        this.timeout = 1500;
      }
    },
    normal() {
      let normal = document.getElementById("normal");
      if (normal.checked == true) {
        this.timeout = 1000;
      }
    },
    hard() {
      let hard = document.getElementById("hard");
      if (hard.checked == true) {
        this.timeout = 400;
      }
    },
    start() {
      this.clearColor();
      this.msg = '';
      this.on = true;
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
      this.intervalId = setInterval(this.gameTurn, this.timeout);
      this.clearColor()
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
      this.on = false;

      if (this.flash == this.count) {
        clearInterval(this.intervalId);
        this.compTurn = false;
        this.clearColor();
        this.on = true;
      }
      if (this.compTurn) {
        this.clearColor();
        setTimeout(() => {
          if (this.order[this.flash] == 1) this.one();
          if (this.order[this.flash] == 2) this.two();
          if (this.order[this.flash] == 3) this.three();
          if (this.order[this.flash] == 4) this.four();
          this.flash++;
        }, 300);
      }
    },
    audioBtn(clip) {
      if (this.noise) {
        let audio = document.getElementById(clip);
        audio.play()
      }
    },
    one() {
     this.audioBtn("clip1");
      this.noise = true;
      let sGreen = document.getElementById("s-green");
      sGreen.style.backgroundColor = "lightgreen";
    },
    two() {
      this.audioBtn("clip2");
      this.noise = true;
      let sRed = document.getElementById("s-red");
      sRed.style.backgroundColor = "red";
    },
    three() {
      this.audioBtn("clip3");
      this.noise = true;
      let sYellow = document.getElementById("s-yellow");
      sYellow.style.backgroundColor = "yellow";
    },
    four() {
      this.audioBtn("clip4");
      this.noise = true;
      let sBlue = document.getElementById("s-blue");
      sBlue.style.backgroundColor = "blue";
    },
    redPress() {
      this.press(2, this.two());
      if (this.on == false) {
        setTimeout(() => {
          this.clearColor();
        }, 400);
      }
    },
    bluePress() {
      this.press(4, this.four());
      if (this.on == false) {
        setTimeout(() => {
          this.clearColor();
        }, 400);
      }
    },
    yellowPress() {
      this.press(3, this.three());
      if (this.on == false) {
        setTimeout(() => {
          this.clearColor();
        }, 400);
      }
    },
    greenPress() {
      this.press(1, this.one());
      if (this.on == false) {
        setTimeout(() => {
          this.clearColor();
        }, 400);
      }
    },
    press(num, change) {
      if (this.on == true) {
        this.playOrder.push(num);
        this.check();
        change;
        if (!this.win) {
          setTimeout(() => {
            this.clearColor();
          }, 400);
        }
      }
    },
    check() {
      if (this.playOrder[this.playOrder.length - 1] !== this.order[this.playOrder.length - 1])
        this.good = false;

      if (this.playOrder.length == 20 && this.good) {
        this.winGame();
      }
      if (this.good == false) {
        this.msg = 'Вы проиграли после ' + this.turnCounter + ' раундов :(';
        this.clearColor();
        this.noise = false
      }

      if (this.count == this.playOrder.length && this.good && !this.win  ) {
        this.count++;
        this.playOrder = [];
        this.compTurn = true;
        this.flash = 0;
        this.turnCounter = this.count;
        this.intervalId = setInterval(this.gameTurn, this.timeout);
      }

    },
    winGame() {
      this.msg = 'Вы победили, поздравляю!';
      this.win = true;
      this.on = false;
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
.game-set{
  text-align: left;
  padding-left: 40px;
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
    padding: 10px 20px;
    margin-right: 10px;
  }
  .btn-start:hover{background-color: orange}
.btn-start:active{background-color: darkorange}
  .count{
    font-size: 36px;
  }
input{
  width: 16px;
  height: 16px;
}
.option{
  font-size: 22px;
}

//media

  @media all and (max-width: 600px){
    #app{
       width: 300px;
      margin: 20px auto;
    }
    .flex-main{
      display: block;
    }
    .game-set{
      padding-left: 0;
      margin-top: 30px;
    }
    h1{
      font-size: 28px;
    }
    .count{
      font-size: 24px;
    }
    .btn-start{
      font-size: 18px;
      margin-right: 0;
      padding: 10px 30px;
    }
  }

@media all and (max-width: 300px){
  #app{
    width: 250px;
    margin: 20px auto;
  }
  .flex-main{
    display: block;
  }
  .game-set{
    padding-left: 0;
    margin-top: 30px;
  }
  h1{
    font-size: 26px;
  }
  .count{
    font-size: 22px;
  }
  .btn-start{
    font-size: 16px;
    margin-right: 0;
    padding: 10px 30px;
  }
  .simon{
    width: 250px;
    height: 250px;
  }
  .sector{
    width: 114px;
    height: 114px;
  }
  h2{
    font-size: 20px;
  }
  .option{
    font-size: 20px;
  }
}
</style>
