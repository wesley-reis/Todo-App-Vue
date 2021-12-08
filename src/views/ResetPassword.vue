<template>
      <div class="mx-8 sm:w-8/12 md:w-5/12 lg:w-4/12 sl:w-3/12">
    <div class="flex items-center justify-center mb-5">
      <img class="-mt-28 opacity-95 w-2/6" src="../assets/logo.png" />
    </div>
        <h3 class="py-2 mb-8 text-purple-300 text-lg font-medium text-center">
            Recuperar senha
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
            ref="resetPasswordForm"
            tag="form"
            @submit.stop.prevent="resetPassword()"
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
                        class="bg-input placeholder-gray-300 text-white font-light rounded focus:outline-none py-3 px-4 block w-full appearance-none leading-normal"
                    >

                    <div
                        v-if="!!errors[0]"
                        class="text-red-700 text-sm mb-2"
                    >
                        {{ errors[0] }}
                    </div>
                </ValidationProvider>

                <ValidationProvider
                    v-slot="{ errors }"
                    :rules="{ required: true, regex: /^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).+$/ }"
                    name="Senha"
                >
                    <input
                        v-model="password"
                        type="password"
                        placeholder="Digite sua senha"
                        class="bg-input placeholder-gray-300 text-white font-light rounded focus:outline-none py-3 px-4 block w-full appearance-none leading-normal"
                    >

                    <div
                        v-if="!!errors[0]"
                        class="text-red-700 text-sm mb-2"
                    >
                        {{ errors[0] }}
                    </div>
                </ValidationProvider>

                <button
                    type="submit"
                    :disabled="spinner.reset_password"
                    class="flex items-center justify-center bg-purple-800 hover:bg-purple-900 transition text-blue-200 font-medium text-sm focus:outline-none rounded py-3 px-4 w-full appearance-none leading-normal"
                >
                    <img
                        v-if="spinner.reset_password"
                        src="@/assets/img/spinner.svg"
                        alt=""
                        class="w-5 h-5 mr-2"
                    >

                    ALETRAR SENHA
                </button>

                <div class="my-4 text-right">
                    <RouterLink
                        :to="{ name: 'login' }"
                        class="text-sm font-light text-gray-300"
                    >
                        Login
                    </RouterLink>
                </div>
            </div>
        </ValidationObserver>
    </div>
</template>

<script>
    import { ValidationObserver, ValidationProvider } from 'vee-validate';
    import messages from '@/utils/messages';

    export default {
        name: 'ResetPassword',

        components: {
            ValidationObserver,
            ValidationProvider,
        },

        data() {
            return {
                email: '',
                password: '',
                token: '',
                response: {
                    color: '',
                    message: '',
                },
                spinner: {
                    reset_password: false,
                },
            };
        },

        beforeRouteEnter(to, from, next) {
            if (!to?.query?.token) {
                next({ name: 'login' });
            }

            next();
        },

        created() {
            this.token = this.$route?.query?.token || '';
        },

        methods: {
            async resetPassword() {
                const validator = await this.$refs.resetPasswordForm.validate();
                if (!validator) { return; }

                this.resetResponse();

                const payload = {
                    email: this.email,
                    password: this.password,
                    token: this.token,
                };

                this.spinner.reset_password = true;

                this.$axios.post('v1/reset-password', payload).then(() => {
                    this.response.color = 'green';
                    this.response.message = 'Senha alterada com sucesso!';

                    this.resetForm();
                }).catch((e) => {
                    const errorCode = e?.response?.data?.error || 'ServerError';
                    this.response.color = 'red';
                    this.response.message = messages[errorCode];
                }).finally(() => {
                    this.spinner.reset_password = false;
                });
            },

            resetResponse() {
                this.response.color = '';
                this.response.message = '';
            },

            resetForm() {
                this.$refs.resetPasswordForm.reset();
                this.email = '';
                this.password = '';
                this.token = '';
            },
        },
    };
</script>
