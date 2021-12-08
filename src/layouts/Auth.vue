<template>
    <div class="bg_layout">
        <div class=" flex items-center justify-center w-full mx-auto">
            <RouterView />
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Auth',

        data() {
            return {
            };
        },

        methods: {},
    };
</script>

<style scoped>

.bg_layout {
    @apply w-screen h-screen flex items-center justify-center bg-gradient-to-t from-green-600 via-green-800 to-green-900;

  background: linear-gradient(#04edb4, #058767);
  background-image: url(../assets/img/bg-layout-Auth.png);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}


</style>
