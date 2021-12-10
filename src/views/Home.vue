<template>
  <div class="w-full sm:w-1/2 lg:w-1/3 mx-auto">

    <div class="flex items-center justify-between mb-8">
      <div class="text-white text-xl font-medium">Lista de tarefas</div>
    </div>

    <form
      class="
        input
        flex
        items-center
        px-4
        h-12
        border-none
        focus:outline-none
        rounded-md
        mb-3
      "
      @submit.stop.prevent="createTodo"
    >
      <input
        v-model="newTodo"
        placeholder="Digite o nome da sua lista ..."
        type="text"
        class="
          input
          placeholder-gray-300
          text-white
          font-light
          focus:outline-none
          block
          w-full
          appearance-none
          leading-normal
          pr-3
          py-2
          px-4
        "
      />

      <button
        type="submit"
        class="
          flex
          justify-center
          items-center
          text-purple-700 text-xs
          font-semibold
          focus:outline-none
        "
      >
        ADICIONAR
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 ml-1"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
          />
        </svg>
      </button>
    </form>

    <div v-if="spinner.get_todos" class="text-center">
      <img src="@/assets/img/spinner.svg" alt="" class="inline-block w-5 h-5" />
    </div>

    <div class="w-full mt-5 pb-60 h-screen overflow-y-scroll">
    <TodoCard
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @afterDeleting="afterDeleting"
    />
    </div>


</div>
 
</template>

<script>
import TodoCard from "@/components/Todos/TodoCard";

export default {
  name: "Home",

  components: {
    TodoCard,
  },

  data() {
    return {
      newTodo: "",
      todos: [],
      spinner: {
        get_todos: false,
      },
    };
  },

  created() {
    this.getTodos();
  },

  methods: {
    getTodos() {
      this.spinner.get_todos = true;
      this.$axios
        .get("v1/todos")
        .then((response) => {
          this.todos = response.data.data.map((o) => ({
            ...o,
            state: "show",
          }));
        })
        .finally(() => {
          this.spinner.get_todos = false;
        });
    },

    createTodo() {
      if (!this.newTodo) {
        return;
      }

      const payload = {
        label: this.newTodo,
      };

      this.$axios.post("v1/todos", payload).then((response) => {
        this.todos.unshift({ ...response.data.data, state: "show" });
        this.newTodo = "";
      });
    },

    afterDeleting(todo) {
      const idx = this.todos.findIndex((o) => o.id === todo.id);
      this.todos.splice(idx, 1);
    },
  },
};
</script>
<style>
::-webkit-scrollbar {
    -webkit-appearance: none;
    width: 7px;
}
::-webkit-scrollbar-thumb {
    border-radius: 4px;
    background-color: rgba(0,0,0,.5);
    -webkit-box-shadow: 0 0 1px rgba(255,255,255,.5);
}
</style>
