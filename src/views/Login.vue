<template>
  <div class="mx-8 sm:w-8/12 md:w-5/12 lg:w-4/12 sl:w-3/12">
    <LoadingScreen :isLoading="isLoading"/>
    <div v-if="!isLoading">
      <LoginMenu />
      <div
        v-if="response.message"
        :class="`rounded bg-transparent border-2 border-${response.color}-800 p-4 mb-4`"
      >
        <h3 :class="`text-sm leading-5 font-medium text-${response.color}-800`">
          {{ response.message }}
        </h3>
      </div>
      <ValidationObserver
        ref="loginForm"
        tag="form"
        @submit.stop.prevent="login()"
      >
        <div class="grid gap-2">
          <ValidationProvider
            v-slot="{ errors }"
            rules="required|email"
            name="E-mail"
          >
            <input
              v-model="email"
              type="text"
              placeholder="Digite seu e-mail"
              class="
                bg-input
                placeholder-gray-300
                text-white
                font-light
                rounded
                focus:outline-none
                py-3
                px-4
                block
                w-full
                appearance-none
                leading-normal
              "
            />

            <div v-if="!!errors[0]" class="text-red-700 text-sm mb-2">
              {{ errors[0] }}
            </div>
          </ValidationProvider>

          <ValidationProvider v-slot="{ errors }" rules="required" name="Senha">
            <input
              v-model="password"
              type="password"
              placeholder="Digite sua senha"
              class="
                bg-input
                placeholder-gray-300
                text-white
                font-light
                rounded
                focus:outline-none
                py-3
                px-4
                block
                w-full
                appearance-none
                leading-normal
              "
            />

            <div v-if="!!errors[0]" class="text-red-700 text-sm mb-2">
              {{ errors[0] }}
            </div>
          </ValidationProvider>

          <button
            type="submit"
            :disabled="spinner.login"
            class="
              flex
              items-center
              justify-center
              bg-purple-800
              hover:bg-purple-900
              transition
              text-blue-200
              font-medium
              text-sm
              focus:outline-none
              rounded
              py-3
              px-4
              w-full
              appearance-none
              leading-normal
            "
          >
            <img
              v-if="spinner.login"
              src="@/assets/img/spinner.svg"
              alt=""
              class="w-5 h-5 mr-2"
            />

            ENTRAR
          </button>

          <div class="my-1 text-right">
            <RouterLink
              :to="{ name: 'forgotPassword' }"
              class="text-sm font-light text-gray-300"
            >
              Esqueci minha senha
            </RouterLink>
          </div>
        </div>
      </ValidationObserver>
    </div>
  </div>
</template>

<script>
import LoginMenu from "@/components/Auth/LoginMenu";
import Cookie from "@/service/cookie";
import { ValidationObserver, ValidationProvider } from "vee-validate";
import messages from "@/utils/messages";
import LoadingScreen from "../components/SplashScreen/LoadingScreen.vue";

export default {
  name: "Login",
  components: {
    LoginMenu,
    ValidationObserver,
    ValidationProvider,
    LoadingScreen
},

  data() {
    return {
      email: "",
      password: "",
      response: {
        color: "",
        message: "",
      },
      spinner: {
        login: false,
      },
      isLoading: true,
    };
  },

  mounted() {
    setTimeout(() => {
      this.isLoading = false;
    }, 3000);
  },

  methods: {
    async login() {
      const validator = await this.$refs.loginForm.validate();
      if (!validator) {
        return;
      }

      const payload = {
        email: this.email,
        password: this.password,
      };

      this.resetResponse();

      this.spinner.login = true;

      this.$axios
        .post("v1/login", payload)
        .then((response) => {
          const token = `${response.data.token_type} ${response.data.access_token}`;
          Cookie.setToken(token);

          this.$store.commit("user/STORE_USER", response.data.data);

          this.$router.push({ name: "index" });
        })
        .catch((e) => {
          this.spinner.login = false;

          const errorCode = e?.response?.data?.error || "ServerError";
          this.response.color = "red";
          this.response.message = messages[errorCode];
        });
    },

    resetResponse() {
      this.response.color = "";
      this.response.message = "";
    },
  },
};
</script>

