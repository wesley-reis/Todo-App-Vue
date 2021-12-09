<template>
  <div id="app">
    <LoadingScreen :isLoading="isLoading" />
    <main v-if="!isLoading">
      <RouterView />
    </main>
  </div>
</template>
<script>
import LoadingScreen from "./components/SplashScreen/LoadingScreen.vue";

export default {
  components: { LoadingScreen },
  data() {
    return { isLoading: true };
  },
  mounted() {
    setTimeout(() => {
      this.isLoading = false;
    }, 3000);
  },
};
</script>
