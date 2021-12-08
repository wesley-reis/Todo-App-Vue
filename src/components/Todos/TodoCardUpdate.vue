<template>
  <ValidationObserver
    ref="todoUpdateForm"
    tag="form"
    @submit.stop.prevent="onSave"
  >
    <div class="input flex items-center rounded-md px-4 h-15 mb-2">
      <ValidationProvider
        v-slot="{ errors }"
        rules="required"
        name="Label"
        class="w-full mr-3"
      >
        <input
          v-model="localLabel"
          ref="label"
          type="text"
          placeholder="Digite o nome da sua lista ..."
          class="
            input
            placeholder-gray-400
            text-white
            font-light
            focus:outline-none
            block
            w-full
            appearance-none
            leading-normal
            pr-3
            py-3
            px-4
          "
        />

        <div v-if="!!errors[0]" class="text-red-500 text-sm mb-2">
          {{ errors[0] }}
        </div>
      </ValidationProvider>

      <div class="flex items-center ml-auto">
        <a
          href=""
          class="text-xs text-purple-200 mr-2 focus:outline-none"
          @click.stop.prevent="onCancel()"
        >
          Cancelar
        </a>

        <button
          type="submit"
          class="
            bg-purple-700
            text-blue-100 text-xs
            font-semibold
            px-2.5
            py-1
            rounded
            hover:bg-purple-800
            focus:outline-none
          "
        >
          OK
        </button>
      </div>
    </div>
  </ValidationObserver>
</template>

<script>
import { ValidationObserver, ValidationProvider } from "vee-validate";

export default {
  name: "TodoCardUpdate",

  components: {
    ValidationObserver,
    ValidationProvider,
  },

  props: {
    todo: {
      type: Object,
      default: () => ({}),
    },
  },

  data() {
    return {
      localLabel: this.todo.label,
    };
  },

  mounted() {
    this.$refs.label.focus();
  },

  methods: {
    onCancel() {
      this.$emit("cancel");
    },

    async onSave() {
      const validator = await this.$refs.todoUpdateForm.validate();
      if (!validator) {
        return;
      }

      const payload = {
        label: this.localLabel,
      };

      this.$axios.put(`v1/todos/${this.todo.id}`, payload).then(() => {
        this.todo.label = this.localLabel;
        this.onCancel();
      });
    },
  },
};
</script>
