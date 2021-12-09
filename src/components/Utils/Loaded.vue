<template>
  <div class="page-loader" v-if="!isloaded">
    <div class="cube bg-green-400"></div>
    <div class="cube bg-blue-400"></div>
    <div class="cube bg-yellow-400"></div>
    <div class="cube bg-red-400"></div>
  </div>
</template>

<script>
export default {
    name:"SplashScreen",
    data:() => {
      return{
        isloaded:false
      }
    },
    mounted(){
      document.onreadystatechange = () => {
        if(document.readyState == "complete"){
          this.isloaded = true;
        }
      }
      
    }

}
</script>

<style scoped>
.page-loader{
  display:flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left:0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, .7);
  z-index: 999;
}

.cube{
  width: 40px;
  height: 40px;
  margin-right: 10px;
}

.cube:first-child{
  animation: left 1s infinite;
}

.cube:last-child{
  animation: right 1s infinite .5s;
}



@keyframes left {
    40%{
      transform: translateX(-60px);
    },
    50%{
      transform: translateX(0);
    }
  }

    @keyframes right {
    40%{
      transform: translateX(60px);
    },
    50%{
      transform: translateX(0);
    }
  }

</style>