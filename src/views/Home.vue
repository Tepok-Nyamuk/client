<template>
  <div class="home newcursor" >
    <!-- <div class="firefly">
    </div> -->
    

     <h2>Create Room</h2>
        <form action="" @submit.prevent="createRoom">
            <label for="">Room Name</label>
            <input type="text" v-model="roomName"> <br>
            <label for="">Number of Player</label>
            <input type="number" v-model="roomPlayers"> <br>
            <input type="submit">
        </form>

        <h2>Join Room</h2>
        <ul v-for="room in roomList" :key="room">
            <a href="" @click.prevent="joinRoom(room)">{{ room }}</a>
        </ul>

    <form v-if='!isLogin' action="" @submit.prevent="login">
        <label for="username">username</label>
        <input v-model="username" type="text">
        <input type="submit">
    </form>


      <!-- <button @click.prevent="addScore">tambah</button> -->
        <!-- <ul v-for="(score,index) in scores" :key="index"> -->
            <!-- <li :key="score.username">{{score.username}}</li> -->
            <!-- <li :key="score.score">{{score.score}}</li> -->
        <!-- </ul> -->

      <button @click="addScore" class="mosquitos" :style= "{top:positionX + 'px', right:positionY + 'px'}" >
      </button>


        <div class="playerBoard">
      <!-- ------------------------------------------ -->

            <div class="players">
              <b-card class="playerScore" style="top:100px; display:flex;padding-bottom:-50px;">
                <b-card-title>player A</b-card-title>
                <h4> score : {{score}}</h4>
                <br>
            </b-card>
            <br>

      <!-- ----------------------------------------- -->
          </div>
        </div>

    
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
      score : 0,
      scores : [],
      myRoom : '',
      roomName : "",
      roomPlayers : 0,
      positionX: '', //0-800
      positionY: '', //0-1400
      roomList : [],
    }
  },
  name: 'home',
  methods:{
    
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
    createRoom() {
          console.log({name : this.roomName, player : this.roomPlayers})
          this.mySocket.emit('create', {name : this.roomName, player : this.roomPlayers}) 
      },
    joinRoom(room) {
        this.mySocket.emit('join', room) 
    }
  },
  created () {
        this.mySocket.on('dari-server', (data) => {
          // console.log(data)
            this.positionX = data.xAxis
            this.positionY = data.yAxis
        })
        this.mySocket.on('send-roomList', (data) => {
            this.roomList = data
        })
        this.mySocket.on('send-loading', (data) => {
            console.log('loading dulu yahhh..')
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

.firefly{
  background-image: url(../img/fire-fly.gif);
  height:100vh;
  background-blend-mode: screen;
}
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
  background-color:transparent;
  border :none;
}

.mosquitos:hover{
  cursor:url(../img/cursor-hover.png),auto;
}
.mosquitos:active{
  cursor:url(../img/hit-effect.png) ,auto;
  height: 200px;
}

/* .custom {
  cursor: url(../img/cursor-hover.png), auto;
} */

/* div {
    cursor:url(../img/cursor-hover.png),auto;
} */

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
