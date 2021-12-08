<template>
    <div class="mx-8 sm:w-8/12 md:w-5/12 lg:w-4/12 sl:w-3/12">
        <LoginMenu />

        <div
            v-if="response.message"
            :class="`rounded-sm bg-${response.color}-100 p-4 mb-4`"
        >
            <h3 :class="`text-sm leading-5 font-medium text-${response.color}-800`">
                {{ response.message }}
            </h3>
        </div>

        <ValidationObserver
            ref="registerForm"
            tag="form"
            @submit.stop.prevent="register()"
        >
            <div class="grid gap-2">
                <div class="flex">
                    <div class="w-1/2 mr-2">
                        <ValidationProvider
                            v-slot="{ errors }"
                            rules="required"
                            name="Primeiro nome"
                        >
                            <input
                                v-model="firstName"
                                type="text"
                                placeholder="Digite seu nome"
                                class="bg-input placeholder-gray-300 text-white font-light rounded focus:outline-none py-3 px-4 block w-full appearance-none leading-normal"
                            >

                            <div
                                v-if="!!errors[0]"
                                class="text-red-700 text-sm mb-2"
                            >
                                {{ errors[0] }}
                            </div>
                        </ValidationProvider>
                    </div>

                    <div class="w-1/2  ml-2">
                        <input
                            v-model="lastName"
                            type="text"
                            placeholder="Digite seu sobrenome"
                            class="bg-input placeholder-gray-300 text-white font-light rounded focus:outline-none py-3 px-4 block w-full appearance-none leading-normal"
                        >
                    </div>
                </div>

                <ValidationProvider
                    v-slot="{ errors }"
                    rules="required|email"
                    name="E-mail"
                >
                    <input
                        v-model="email"
                        type="text"
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
                    :disabled="spinner.register"
                    class="flex items-center justify-center bg-purple-800 hover:bg-purple-900 transition text-blue-200 font-medium text-sm focus:outline-none rounded py-3 px-4 w-full appearance-none leading-normal"
                >
                    <img
                        v-if="spinner.register"
                        src="@/assets/img/spinner.svg"
                        alt=""
                        class="w-5 h-5 mr-2"
                    >

                    REGISTRAR
                </button>
            </div>
        </ValidationObserver>
    </div>
</template>

<script>
    import LoginMenu from '@/components/Auth/LoginMenu';
    import { ValidationObserver, ValidationProvider } from 'vee-validate';
    import messages from '@/utils/messages';

    export default {
        name: 'Register',

        components: {
            LoginMenu,
            ValidationObserver,
            ValidationProvider,
        },

        data() {
            return {
                firstName: '',
                lastName: '',
                email: '',
                password: '',
                response: {
                    color: '',
                    message: '',
                },
                spinner: {
                    register: false,
                },
            };
        },

        methods: {
            async register() {
                const validator = await this.$refs.registerForm.validate();
                if (!validator) { return; }

                this.resetResponse();

                const payload = {
                    first_name: this.firstName,
                    last_name: this.lastName,
                    email: this.email,
                    password: this.password,
                };

                this.spinner.register = true;

                this.$axios.post('v1/register', payload).then(() => {
                    this.response.color = 'green';
                    this.response.message = 'Seu cadastro foi feito com sucesso.';

                    this.resetForm();
                }).catch((e) => {
                    const errorCode = e?.response?.data?.error || 'ServerError';
                    console.log(errorCode);
                    this.response.color = 'red';
                    this.response.message = messages[errorCode];
                }).finally(() => {
                    this.spinner.register = false;
                });
            },

            resetResponse() {
                this.response.color = '';
                this.response.message = '';
            },

            resetForm() {
                this.$refs.registerForm.reset();
                this.firstName = '';
                this.lastName = '';
                this.email = '';
                this.password = '';
            },
        },
    };
</script>
