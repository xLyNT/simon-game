<template>
  <div id="app">
    <div class="container">
      <div class="simon">
        <div class="tile tile-one" @click="tileClick"></div>
        <div class="tile tile-two" @click="tileClick"></div>
        <div class="tile tile-three" @click="tileClick"></div>
        <div class="tile tile-four" @click="tileClick"></div>
      </div>
      <div class="diff-selector">
        <h2>Счет:{{this.score}}</h2>
        <h3>Уровень сложности</h3>
        <input type="radio" id="easy" name="diff" v-model="difficulty" value="1500"><label for="easy" >Легко</label>
        <input type="radio" id="med" name="diff" v-model="difficulty" value="1000"><label for="med">Средне</label>
        <input type="radio" id="high" name="diff" v-model="difficulty" value="400"><label for="high">Сложно</label>
      </div>
      <div class="btn" @click="playGame">Играть</div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
      difficulty:0,
      sounds: [
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound1.mp3"),
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound2.mp3"),
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound3.mp3"),
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound4.mp3"
      )
    ],
      isRound:false,
      isPlaying:false,
      tilesList:[],
      tileToClick:0,
      tileToClickIndex:0,
      roundCount:1,
      score:0
    }
  },
  components: {
    
  },
  methods:{
   playTile(index,delay){ // Проигрывание одной клетки
     const colorTiles = document.querySelectorAll('.tile')
     
    setTimeout(() => {
     
      colorTiles[index].classList.add(colorTiles[index].classList[1] + '-light');
      this.sounds[index].play();
      setTimeout(() => {
        
        colorTiles[index].classList.remove(colorTiles[index].classList[1] + '-light');

      }, this.difficulty)
    },delay)
  },
  playRound(round){ // Проигрывание раунда
    let delay = +this.difficulty;
    let random = Math.floor(Math.random() * 4);
    this.tilesList.push(random);
  for(let i = 0; i < round; i++){
    this.playTile(this.tilesList[i],delay);
    delay += +this.difficulty + 500;

    if(i === round - 1){
      setTimeout(() => {
         this.isRound = false;
      },delay)
    }
  }
    this.roundCount++;
    this.tileToClick = this.tilesList[0];
    this.tileToClickIndex = 0;
  },
  playGame(){// Старт игры
      if(!this.difficulty || this.isRound) return;
      this.isRound = true;
      this.isPlaying = true;
      this.playRound(this.roundCount);
    },
    tileClick(e){
      const tiles = Array.from(document.querySelectorAll('.tile'));
      if(this.isRound) return;
      if((tiles.indexOf(e.target) === this.tileToClick )&& this.isPlaying){
        const currentTile = tiles[this.tileToClick]
        currentTile.classList.toggle(currentTile.classList[1] + '-light');
        this.sounds[this.tileToClick].play();
        setTimeout(() => {
          currentTile.classList.toggle(currentTile.classList[1] + '-light');
        }, 200)
        this.score++;
        if(this.tileToClickIndex < this.tilesList.length - 1){
          this.tileToClick = this.tilesList[this.tileToClickIndex + 1];
          this.tileToClickIndex += 1;
        }else{
          setTimeout(() => {this.playRound(this.roundCount)},this.difficulty);
        }
      }else if(this.isRound){
          return;
        }else if(this.isPlaying){
        alert(`Игра окончена, счет:${this.score}`);
        this.score = 0;
        this.isPlaying = false;
        this.roundCount = 1;
        this.tilesList = [];
        this.tileToClick = 0;
        this.tileToClickIndex = 0;
      }
      
      }
    }
}
</script>

<style>
@import 'css/style.css'
</style>
