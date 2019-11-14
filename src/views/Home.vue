<template>
  <div class="home newcursor" >
      <div class="mosquitos"></div>
      <div class="mosquitos2"></div>
  </div>
</template>

<script>

export default {
  name: 'home',
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
}

.mosquitos{
  position: absolute;
  display:block;
  top:100px;
  right:100px;
  width:130px;
  height: 120px;
  background-image: url(../img/mosquito.png);
  background-repeat: no-repeat;
  cursor:url(../img/cursor-hover.png),auto;
}
.mosquitos:active{
  cursor:url(../img/hit-effect.png) ,auto;
  height: 200px;
}
.mosquitos2{
  position: absolute;
  display:block;
  top:80vh;
  right:10vw;
  width:150px;
  height: 150px;
  background-image: url(../img/mosquito.png);
  background-repeat: no-repeat;
  cursor:url(../img/cursor-hover.png),auto;
}

.custom {
  cursor: url(../img/cursor.png), auto;
}

div {
    cursor:url(../img/cursor.png),auto;
}

</style>
