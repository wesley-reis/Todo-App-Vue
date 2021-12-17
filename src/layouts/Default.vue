<template>
  <div>
    <TheHeader />
    <div class="bg_layout px-3 py-10">
      <RouterView />
    </div>
  </div>
</template>

<script>
import TheHeader from "@/components/Partials/TheHeader";

export default {
  name: "Default",

  components: {
    TheHeader,
  },

  data() {
    return {};
  },

  methods: {},
};
</script>
<style scoped>
.bg_layout {
  @apply w-screen h-screen fixed;
  background: linear-gradient(#04edb4, #058767);
  background-image: url(../assets/img/bg-layout-dash-two.png);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
