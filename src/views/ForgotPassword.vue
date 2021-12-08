<template>
  <div class="mx-8 sm:w-8/12 md:w-5/12 lg:w-4/12 sl:w-3/12">
    <div class="flex items-center justify-center mb-5">
      <img class="-mt-28 opacity-95 w-2/6" src="../assets/logo.png" />
    </div>
    <h3 class="py-2 mb-8 text-purple-300 text-lg font-medium text-center">
      Esqueci minha senha
    </h3>

    <div
      v-if="response.message"
      :class="`rounded-sm bg-${response.color}-100 p-4 mb-4`"
    >
      <h3 :class="`text-sm leading-5 font-medium text-${response.color}-800`">
        {{ response.message }}
      </h3>
    </div>

    <ValidationObserver
      ref="forgotPasswordForm"
      tag="form"
      @submit.stop.prevent="forgotPassword()"
    >
      <div class="grid gap-2">
        <ValidationProvider
          v-slot="{ errors }"
          rules="required|email"
          name="E-mail"
        >
          <input
            v-model="email"
            type="email"
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

        <button
          type="submit"
          :disabled="spinner.forgot_password"
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
            v-if="spinner.forgot_password"
            src="@/assets/img/spinner.svg"
            alt=""
            class="w-5 h-5 mr-2"
          />

          RECUPERAR SENHA
        </button>

        <div class="my-1 text-right">
          <RouterLink :to="{ name: 'login' }" class="text-sm font-light text-gray-300">
            Login
          </RouterLink>
        </div>
      </div>
    </ValidationObserver>
  </div>
</template>

<script>
import { ValidationObserver, ValidationProvider } from "vee-validate";
import messages from "@/utils/messages";

export default {
  name: "ForgotPassword",

  components: {
    ValidationObserver,
    ValidationProvider,
  },

  data() {
    return {
      email: "",
      response: {
        color: "",
        message: "",
      },
      spinner: {
        forgot_password: false,
      },
    };
  },

  methods: {
    async forgotPassword() {
      const validator = await this.$refs.forgotPasswordForm.validate();
      if (!validator) {
        return;
      }

      this.resetResponse();

      const payload = {
        email: this.email,
      };

      this.spinner.forgot_password = true;

      this.$axios
        .post("v1/forgot-password", payload)
        .then(() => {
          this.response.color = "green";
          this.response.message =
            "Enviamos um e-mail com instruções de recuperação de senha.";

          this.resetForm();
        })
        .catch((e) => {
          const errorCode = e?.response?.data?.error || "ServerError";
          this.response.color = "red";
          this.response.message = messages[errorCode];
        })
        .finally(() => {
          this.spinner.forgot_password = false;
        });
    },

    resetResponse() {
      this.response.color = "";
      this.response.message = "";
    },

    resetForm() {
      this.$refs.forgotPasswordForm.reset();
      this.email = "";
    },
  },
};
</script>
