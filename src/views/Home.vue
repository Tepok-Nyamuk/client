<template>
  <div class="home newcursor" >
     
      <button @click="addScore" class="mosquitos" :style= "{top:positionX + 'px', right:positionY + 'px'}" >
      </button>
      

        <div class="playerBoard">

          <div class="players">
            <b-card class="playerScore" style="top:100px;">
              <b-card-title>{{player}}</b-card-title>
              <h5> score : {{score}}</h5>
              <br>
            </b-card>
            
            <br>
          </div>

          <div class="players">
            <b-card class="playerScore" style="top:100px;">
              <b-card-title>{{player}}</b-card-title>
              <h5> score : {{score}}</h5>
              <br>
            </b-card>
            
            <br>
          </div>

          <div class="players">
            <b-card class="playerScore" style="top:100px;">
              <b-card-title>{{player}}</b-card-title>
              <h5> score : {{score}}</h5>
              <br>
            </b-card>
            
            <br>
          </div>

          <div class="players">
            <b-card class="playerScore" style="top:100px;">
              <b-card-title>{{player}}</b-card-title>
              <h5> score : {{score}}</h5>
              <br>
            </b-card>
            
            <br>
          </div>

        </div>
    
  </div>
</template>

<script>

export default {
  data(){
    return {
      score: 0,
      positionX: 800, //0-800
      positionY: 1400, //0-1400
      player:'player A'
    }
  },
  name: 'home',
  methods:{
    addScore(){
      this.score ++
    }
  },
  components: {
  }
}
</script>

<style>
.home{
  height: 100vh;
  background-image: url(../img/background.jpg);
  display:block;
  cursor:url(../img/cursor.png),auto;
  background-repeat: no-repeat;
  background-size:cover;
  font-family: 'Luckiest Guy';
  font-size: 12px;
}

button:focus { outline: none; }

.mosquitos{
  position: absolute;
  display:block;
  /* top:100px;
  right:100px; */
  width:130px;
  height: 120px;
  background-image: url(../img/mosquito.png);
  background-repeat: no-repeat;
  cursor:url(../img/cursor-hover.png),auto;
  background-color:transparent;
  border :none;
}
.mosquitos:active{
  cursor:url(../img/hit-effect.png) ,auto;
  height: 200px;
}

.custom {
  cursor: url(../img/cursor.png), auto;
}

div {
    cursor:url(../img/cursor.png),auto;
}

.card-body {
    -ms-flex: 1 1 auto;
    flex: 1 1 auto;
    padding: 1rem;
}

.playerBoard {
  background-image: url(../img/boardplayer.png);
  height: 100%;
  background-repeat: no-repeat;
}

.playerScore {
  width: 200px;
  height: auto;
  text-align: left;
  margin-left: 100px;
}

</style>
