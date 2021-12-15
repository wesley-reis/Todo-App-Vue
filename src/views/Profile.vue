<template>
  <div class="w-full sm:w-1/2 lg:w-1/3 mx-auto">
    <div class="flex items-center justify-between mb-8">
      <div class="text-white text-xl font-medium">Meu perfil</div>
    </div>
    <div
      v-if="response.message"
      :class="`rounded-sm bg-${response.color}-300 border-b-2 border-${response.color}-900 p-4 mb-4`"
    >
      <h3 :class="`text-sm leading-5 font-medium text-${response.color}-800`">
        {{ response.message }}
      </h3>
    </div>

    <ValidationObserver
      ref="profileForm"
      tag="form"
      @submit.stop.prevent="update"
    >
      <div class="flex flex-col justify-center items-center mb-5 gap-2">
        <img
          v-if="imageSrc"
          :src="imageSrc"
          class="rounded-full border-2 border-purple-500 w-24 h-24"
          alt="userFoto"
        />
        <label class="cursor-pointer text-center">
          <span
            class="flex justify-center items-center text-xs text-purple-800"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-4 w-4 mr-1"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
              />
            </svg>
            <span v-if="imageSrc">Alterar Foto</span>
            <span v-else>Incluir Foto</span>
          </span>
          <input
            type="file"
            @change="getAvatar"
            class="hidden"
            id="customFile"
          />
          {{ this.user.avatar }}
        </label>
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <ValidationProvider
            v-slot="{ errors }"
            rules="required"
            name="Primeiro nome"
            tag="div"
          >
            <label
              for="firstName"
              class="block text-sm text-gray-600 font-medium mb-2"
            >
              Primeiro nome
            </label>

            <input
              id="firstName"
              v-model="firstName"
              type="text"
              placeholder="Digite seu nome"
              class="
                input
                placeholder-gray-400
                text-white
                font-light
                border border-green-900
                focus:outline-none focus:border-purple-800
                rounded-sm
                py-3
                px-4
                block
                w-full
                appearance-none
                leading-normal
              "
            />

            <div v-if="!!errors[0]" class="text-red-500 text-sm mb-2">
              {{ errors[0] }}
            </div>
          </ValidationProvider>
        </div>

        <div>
          <label
            for="lastName"
            class="block text-sm text-gray-600 font-medium mb-2"
          >
            Sobrenome
          </label>

          <input
            id="lastName"
            v-model="lastName"
            type="text"
            placeholder="Digite seu sobrenome"
            class="
              input
              placeholder-gray-400
              text-white
              font-light
              border border-green-900
              focus:outline-none focus:border-purple-800
              rounded-sm
              py-3
              px-4
              block
              w-full
              appearance-none
              leading-normal
            "
          />
        </div>

        <div>
          <ValidationProvider
            v-slot="{ errors }"
            rules="required|email"
            name="Email"
            tag="div"
          >
            <label
              for="email"
              class="block text-sm text-gray-600 font-medium mb-2"
            >
              E-mail
            </label>

            <input
              id="email"
              v-model="email"
              type="text"
              placeholder="Digite seu e-mail"
              class="
                input
                placeholder-gray-400
                text-white
                font-light
                border border-green-900
                focus:outline-none focus:border-purple-800
                rounded-sm
                py-3
                px-4
                block
                w-full
                appearance-none
                leading-normal
              "
            />

            <div v-if="!!errors[0]" class="text-red-500 text-sm mb-2">
              {{ errors[0] }}
            </div>
          </ValidationProvider>
        </div>

        <div>
          <ValidationProvider
            v-slot="{ errors }"
            :rules="{ regex: /^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).+$/ }"
            name="Senha"
          >
            <label
              for="password"
              class="block text-sm text-gray-600 font-medium mb-2"
            >
              Senha
            </label>

            <input
              id="password"
              v-model="password"
              type="password"
              placeholder="Digite sua senha"
              class="
                input
                placeholder-gray-400
                text-white
                font-light
                border border-green-900
                focus:outline-none focus:border-purple-800
                rounded-sm
                py-3
                px-4
                block
                w-full
                appearance-none
                leading-normal
              "
            />

            <div v-if="!!errors[0]" class="text-red-500 text-sm mb-2">
              {{ errors[0] }}
            </div>
          </ValidationProvider>
        </div>

        <div class="col-span-2 text-right">
          <button
            type="submit"
            :disabled="spinner.update_user"
            class="
              inline-flex
              items-center
              justify-center
              bg-purple-800
              hover:bg-purple-900
              text-purple-100
              font-medium
              text-sm
              focus:outline-none
              rounded-md
              py-3
              px-4
              inline-block
              appearance-none
              leading-normal
            "
          >
            <img
              v-if="spinner.update_user"
              src="@/assets/img/spinner.svg"
              alt=""
              class="w-5 h-5 mr-2"
            />

            SALVAR
          </button>
        </div>
      </div>
    </ValidationObserver>
  </div>
</template>

<script>
import { mapState, mapMutations } from "vuex";
import { ValidationObserver, ValidationProvider } from "vee-validate";
import messages from "@/utils/messages";

export default {
  name: "Profile",

  components: {
    ValidationObserver,
    ValidationProvider,
  },

  data() {
    return {
      firstName: "",
      lastName: "",
      email: "",
      password: "",
      avatar: "",
      response: {
        color: "",
        message: "",
      },
      spinner: {
        update_user: false,
      },
     imageSrc:null
    };
  },

  computed: {
    ...mapState({
      user: (state) => state.user.user,
    }),
  },

  created() {
    this.firstName = this.user.first_name;
    this.lastName = this.user.last_name;
    this.email = this.user.email;
    this.avatar = this.user.avatar;
  },

  methods: {
    ...mapMutations("user", ["STORE_USER"]),

    getAvatar($evt) {
      let file = $evt.target.files[0];

      this.user.avatar = URL.createObjectURL(file);
      this.imageSrc = URL.createObjectURL(file);
    },

    async update() {
      const validator = await this.$refs.profileForm.validate();
      if (!validator) {
        return;
      }

      const payload = {
        first_name: this.firstName,
        last_name: this.lastName,
        email: this.email,
        avatar: this.avatar,
      };

      if (this.password) {
        payload.password = this.password;
      }

      this.spinner.update_user = true;
      this.$axios
        .post("v1/me", payload)
        .then((response) => {
          this.response.color = "green";
          this.response.message = "Seus dados foram atualizados com sucesso!";

          this.STORE_USER(response.data.data);
        })
        .catch((e) => {
          const errorCode = e?.response?.data?.error || "ServerError";
          this.response.color = "red";
          this.response.message = messages[errorCode];
        })
        .finally(() => {
          this.spinner.update_user = false;
        });
    },
  },
};
</script>
<style>
.input {
  background: #048968;
}
</style>