<template>
  <div :class="{ loader: true, fadeout: !isLoading }">
  </div>
</template>

<script>
export default {
  name: "LoadingScreen",
  props: {
    isLoading:Boolean,
  }
};
</script>

<style scoped>
.loader {
  background-image: url(../../assets/img/bg-layout-init.png);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  bottom: 0;
  color: white;
  display: block;
  font-size: 32px;
  left: 0;
  overflow: hidden;
  padding-top: 10vh;
  position: fixed;
  right: 0;
  text-align: center;
  top: 0;
}

.fadeout {
  animation: fadeout .5s forwards;
}

@keyframes fadeout {
  to {
    opacity: 0;
    visibility: hidden;
  }
}
</style>
