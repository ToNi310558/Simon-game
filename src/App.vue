<template>
  <div id="app">

    <header>
      <h1>Саймон Говорит</h1>
    </header>

    <main class="flex-main">
      <div class="simon">
        <div class="sector s-green"></div>
        <div class="sector s-red"></div>
        <div class="sector s-yellow"></div>
        <div class="sector s-blue"></div>
      </div>

      <div class="game">
        <div class="game-info">
          <span class="count">Раунд:</span>
          <span class="count" id="counter">{{turnCounter}}</span><br>
          <button class="btn-start" value="start" @click="start">Старт</button>
        </div>
        <div class="game-options">
          <h2>Уровень сложности:</h2>
            <input type="radio"  name="option" @click="easy">Легкий<br>
            <input type="radio" name="option" id="normal" @click="normal" checked>Средний<br>
            <input type="radio" name="option" @click="hard">Сложный<br>
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
      Timeout: 1000,
      win: false,
      flash: 0,
      intervalId: 0,
      count: 0,
      turnCounter: 0,
      good: true,
      compTurn: false
    }
  },
  methods: {
    easy() {
      if (this.checked == true)
        this.Timeout(1500)
      console.log(1500)
    },
    normal() {
      if (this.checked == true)
        this.Timeout(1000)
      console.log(1000)
    },
    hard() {
      if (this.checked == true)
        this.Timeout(400)
      console.log(400)
    },
    start() {
      this.clearColor();
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
      for (let i = 0; i<20; i++) {
        this.order.push(Math.floor(Math.random() *4) +1);
      }
      this.compTurn = true
      this.intervalId = setInterval(this.gameTurn, this.Timeout)
    },
    clearColor() {

    },
    gameTurn() {
      if (this.flash == this.count) {
        clearInterval(this.intervalId);
        this.compTurn = false;
        this.clearColor()
      };
      if (this.compTurn) {
        this.clearColor();
        setTimeout(() => {
          if (this.order[this.flash] == 1) this.one();
        }, 1)
      }
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
    background-color: blue;
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
