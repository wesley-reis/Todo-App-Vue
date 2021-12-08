<template>
    <div class="block">
      <div class="flex items-center justify-center mb-5">
        <img class="-mt-28 opacity-95 w-2/6" src="../../assets/logo.png"/>
      </div>

      <div class="flex items-center justify-center mb-8">
        <RouterLink
            :to="{ name: 'login' }"
            class="px-2 py-1 border-b-2 mx-4"
            :class="{ 'border-purple-500 text-white': $route.name === 'login', 'border-none text-gray-400': $route.name !== 'login' }"
        >
          Login
        </RouterLink>

        <RouterLink
            :to="{ name: 'register' }"
            class="px-2 py-1 border-b-2 mx-4"
            :class="{ 'border-purple-500 text-white': $route.name === 'register', 'border-none text-gray-400': $route.name !== 'register' }"
        >
          Registrar
        </RouterLink>
      </div>

    </div>
</template>

<script>
    export default {
        name: 'LoginMenu',

        data() {
            return {
            };
        },

        methods: {},
    };
</script>
