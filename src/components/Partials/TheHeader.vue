<template>
  <div class="header flex items-center justify-between h-16 px-5">
    <div class="flex items-center cursor-pointer">
      <router-link :to="{ name: 'index' }" class="flex items-center">
        <img src="../../assets/logo.png" width="40" alt="logo todo list" />
        <span class="text-white font-semibold ml-2">Todo list</span>
      </router-link>
    </div>

    <div class="flex items-center">
      <TwDropdown naked no-padding class="text-white">
        <template v-slot:button-content>
          <svg
            class="h-6 w-6 mr-1"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M5.12104 17.8037C7.15267 16.6554 9.4998 16 12 16C14.5002 16 16.8473 16.6554 18.879 17.8037M15 10C15 11.6569 13.6569 13 12 13C10.3431 13 9 11.6569 9 10C9 8.34315 10.3431 7 12 7C13.6569 7 15 8.34315 15 10ZM21 12C21 16.9706 16.9706 21 12 21C7.02944 21 3 16.9706 3 12C3 7.02944 7.02944 3 12 3C16.9706 3 21 7.02944 21 12Z"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>

          {{ user.first_name }}
        </template>

        <TwDropdownItem :to="{ name: 'profile' }" class="cursor-pointer">
          <svg
            class="
              mr-3
              h-5
              w-5
              text-gray-500
              group-hover:text-gray-500
              group-focus:text-gray-500
            "
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M16 7C16 9.20914 14.2091 11 12 11C9.79086 11 8 9.20914 8 7C8 4.79086 9.79086 3 12 3C14.2091 3 16 4.79086 16 7Z"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M12 14C8.13401 14 5 17.134 5 21H19C19 17.134 15.866 14 12 14Z"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>

          Meu perfil
        </TwDropdownItem>

        <TwDropdownItem v-on:click="logout" class="cursor-pointer">
          <svg
            class="
              mr-3
              h-5
              w-5
              text-gray-500
              group-hover:text-gray-500
              group-focus:text-gray-500
            "
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M17 16L21 12M21 12L17 8M21 12L7 12M13 16V17C13 18.6569 11.6569 20 10 20H6C4.34315 20 3 18.6569 3 17V7C3 5.34315 4.34315 4 6 4H10C11.6569 4 13 5.34315 13 7V8"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>

          Logout
        </TwDropdownItem>
      </TwDropdown>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import TwDropdown from "@/components/Utils/TwDropdown";
import TwDropdownItem from "@/components/Utils/TwDropdownItem";
import Cookie from "@/service/cookie"
export default {
  name: "TheHeader",

  components: {
    TwDropdown,
    TwDropdownItem,
  },

  data() {
    return {};
  },

  computed: {
    ...mapState({
      user: (state) => state.user.user,
    }),
  },

  methods: {
    logout()
    {
      Cookie.deleteToken();
      this.$router.push({name:'login'});
    }
  },
};
</script>
<style scoped>
  .header{
    background: #048968;
  }
</style>
