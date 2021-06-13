<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players
        v-bind:isWinner="isWinner"
        v-bind:activePlayer="activePlayer"
        v-bind:currentScore="currentScore"
        v-bind:scoresPlayer="scoresPlayer"
      />
      <controls
        v-bind:isPlaying="isPlaying"
        v-bind:finalScore="finalScore"
        v-on:handleHoldScore="handleHoldScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
      />

      <dices v-bind:dices="dices" />
      <prop-rule
        v-bind:isOpenPopup="isOpenPopup"
        v-on:handleConfirm="handleConfirm"
      />
    </div>
  </div>
</template>

<script>
import players from "./components/Players.vue";
import dices from "./components/Dices.vue";
import controls from "./components/Controls.vue";
import propRule from "./components/PropRule.vue";
export default {
  name: "App",
  components: {
    players,
    dices,
    controls,
    propRule
  },
  data() {
    return {
      isPlaying: false,
      activePlayer: 0,
      scoresPlayer: [0, 0],
      currentScore: 0,
      dices: [1, 4],
      finalScore: 30,
      isOpenPopup: false,
    };
  },
  methods: {
    nextPlayer(){
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleNewGame() {
      this.isOpenPopup = true;
    
    },
    handleRollDice() {
      if(this.isPlaying){
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1
      
        this.dices = [dice1, dice2];
        if(dice1 === 1 || dice2 === 1){
          let activePlayer = this.activePlayer;
          setTimeout(function(){
            alert(`Player ${activePlayer +1} has been got number of dice is 1. I'm sorry about that`);

          }, 100)
          this.nextPlayer()
        }
        else {
          this.currentScore = this.currentScore + dice1 + dice2;
        }
          
      }else {
        alert("Please press NewGame button")
      }
    },
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.dices = [6,6];
      this.scoresPlayer = [0,0];
      this.currentScore = 0;
    },
    handleHoldScore() {
      if(this.isPlaying){
        let {scoresPlayer, activePlayer, currentScore} = this;
        let scoreOld = scoresPlayer[activePlayer];
        this.$set(this.scoresPlayer, activePlayer, scoreOld + currentScore);
        if(!this.isWinner){
          this.nextPlayer();
        }
      }
      else{
        alert("Please press NewGame button")
      }
    },
    handleChangeFinalScore(e) {
      var number = parseInt(e.target.value);
      if(isNaN(number)){
        this.finalScore = '';
      }else{
        this.finalScore = number;
      }
    },
  },
  computed: {
    isWinner() {
      let { scoresPlayer, finalScore } = this;
      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
        this.isPlaying = false;
        return true;
      }
      return false;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("./assets/image/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>
