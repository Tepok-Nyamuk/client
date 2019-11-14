<template>
  <div class="home newcursor" >
    score : {{score}}
    <form v-if='!isLogin' action="" @submit.prevent="login">
        <label for="username">username</label>
        <input v-model="username" type="text">
        <input type="submit">
    </form>
    <form v-if="isLogin" action="" @submit.prevent="sendMessage">
        <label for="message">message</label>
        <input type="text" v-model="message">
        <input type="submit" name="send" id="" value="send">
    </form>

      <!-- <button @click.prevent="addScore">tambah</button> -->
        <ul v-for="(score,index) in scores" :key="index">
            <li :key="score.username">{{score.username}}</li>
            <li :key="score.score">{{score.score}}</li>
        </ul>
      <button @click="addScore" class="mosquitos" :style= "{top:positionX + 'px', right:positionY + 'px'}" >
      </button>
        <!-- <div class="playerBoard">

          <div class="players">
            <b-card class="playerScore" style="top:100px;">
              <b-card-title>{{player}}</b-card-title>
              <h5> score : {{score}}</h5>
              <br>
            </b-card>
            
            <br>
          </div>
        </div> -->
    
  </div>
</template>

<script>

export default {
  data(){
    return {
      score: 0,
      username : '',
      mySocket : io("http://localhost:3000"),
      isLogin : false,
      messages : [],
      message : '',
      roomA : [],
      roomB : [],
      score : 0,
      scores : [],  
      roomA : false,
      roomB : false,
      myRoom : '',  
      positionX: '', //0-800
      positionY: '', //0-1400
    }
  },
  name: 'home',
  methods:{
    addScore(){
      this.score ++
    },
    login() {            
      console.log('login guyss')
            this.mySocket.emit('user-login', this.username )        
            this.isLogin = true
    },
    sendMessage() {
        console.log('send')
        this.mySocket.emit('send-message', this.message)   
        //this.mySocket.in(room)        
    },
    addScore() {
        this.score ++
        // this.mySocket.emit('send-score', this.score)
        this.mySocket.emit('send-score', {room : this.myRoom, score : this.score} )
    },
    activRoomA() {
        this.roomA = true;
        this.mySocket.emit('create', 'roomA')
        this.myRoom = 'roomA'
    },
    activRoomB() {
        this.roomB = true;
        this.mySocket.emit('create', 'roomB')
        this.myRoom = "roomB"
    }
  },
  created () {
        this.mySocket.on('dari-server', (data) => {
          // console.log(data)
            this.positionX = data.xAxis
            this.positionY = data.yAxis
        })
        this.mySocket.on('all-score', data => {
            console.log('data=================', data)
            if(this.scores.length === 0 ) {  
                let obj = {
                    username : data.message.username,
                    score : data.message.score.score
                }          
                this.scores.push(obj)
                console.log('this.scores', this.scores)
            } else {
                // if (data.username === this.username) {
                //     this.score = data.score
                // }
                for(let i=0; i<this.scores.length; i++) {
                    if(this.scores[i].username == data.message.username) {
                        let newScore = this.scores[i].score ++
                        this.$set(this.scores[i].score, newScore)
                        console.log('this.scores', this.scores)
                        break
                    }
                }
                this.scores.push(data)
            }
        })
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
