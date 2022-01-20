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
        <h2>Уровень сложности</h2>
        <input type="radio" id="easy" name="diff" v-model="difficulty" value="1500"><label for="easy" >Легко</label>
        
        <input type="radio" id="med" name="diff" v-model="difficulty" value="1000"><label for="med">Средне</label>
        <input type="radio" id="high" name="diff" v-model="difficulty" value="400"><label for="high">Сложно</label>
      </div>
      <div class="btn" @click="playGame">Play</div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
      difficulty:'',
      sounds: [
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound1.mp3"),
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound2.mp3"),
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound3.mp3"),
      new Audio("https://s3.amazonaws.com/freecodecamp/simonSound4.mp3"
      )
    ],
      isPlaying:false,
      tilesList:[],
      tileToClick:0
    }
  },
  components: {
    
  },
  methods:{
   playTile(tile,index){
       tile.classList.toggle(tile.classList[1]+'-light');
       this.sounds[index].play();
       setTimeout(()=> {
         tile.classList.toggle(tile.classList[1]+'-light');       
       }, +this.difficulty);
  },
  getTile(){
    if(!this.tilesList.length){
      this.tilesList.push(Math.round(Math.random() * 3));
    }
    const tiles = document.querySelectorAll('.tile');
    this.playTile(tiles[this.tilesList[0]],this.tilesList[0],this.difficulty);
    
  },
  playGame(){
      this.getTile();
    },
    tileClick(e){
      const tiles = Array.from(document.querySelectorAll('.tile'));
      console.log(this.tileToClick)
      if(this.tileToClick === tiles.indexOf(e.target)){

      e.target.classList.toggle(e.target.classList[1]+'-light');
       this.sounds[tiles.indexOf(e.target)].play();
       setTimeout(()=> {
         e.target.classList.toggle(e.target.classList[1]+'-light');       
       }, 1500);
      }
    }
  }
}
</script>

<style>
@import 'css/style.css'
</style>
