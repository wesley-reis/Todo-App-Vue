<template>
  <div class="w-full sm:w-3/5 lg:w-2/4 xl:w-2/5 mx-auto">
    <div v-if="spinner.get_todo" class="text-center">
      <img
        src="@/assets/img/spinner.svg"
        alt="spinner"
        class="inline-block w-5 h-5"
      />
    </div>

    <template v-else>
      <div class="flex items-center mb-8">
        <RouterLink :to="{ name: 'index' }" class="-ml-2">
          <svg
            class="h-7 w-7 text-purple-500 mr-2"
            viewBox="0 -7 30 40"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              class="cls-1"
               fill-rule="evenodd"
              clip-rule="evenodd"
              d="M16,31A15,15,0,1,1,31,16,15,15,0,0,1,16,31ZM16,3A13,13,0,1,0,29,16,13,13,0,0,0,16,3Z"
               fill="currentColor"
            />
            <path
              class="cls-1"
               fill-rule="evenodd"
              clip-rule="evenodd"
              d="M19.4,22.8l-8-6a1,1,0,0,1,0-1.6l8-6,1.2,1.6L13.67,16l6.93,5.2Z"
               fill="currentColor"
            />
          </svg>
        </RouterLink>

        <div class="text-white text-2xl font-medium">
          {{ todo.label }}
        </div>
      </div>

      <form
        class="input flex items-center px-4 h-15 rounded-md mb-3"
        @submit.stop.prevent="addTask"
      >
        <input
          v-model="newTask"
          placeholder="Adicione um novo item ..."
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
            py-3
            px-4
          "
        />

        <button
          class="
            flex
            items-center
            justify-center
            text-purple-800 text-xs
            font-semibold
            focus:outline-none
          "
          type="submit"
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

      <div
        v-if="todo.tasks.length"
        class="w-full mt-5 pb-60 shadow-2xl h-screen overflow-y-scroll"
      >
        <TodoTaskCard
          v-for="task in todo.tasks"
          :key="task.id"
          :task="task"
          @afterDeleting="afterDeleting"
        />
      </div>

      <div v-else class="text-center text-lg text-purple-800">
        Você ainda não tem nenhuma tarefa.
      </div>
    </template>
  </div>
</template>

<script>
import TodoTaskCard from "@/components/Todos/TodoTaskCard";

export default {
  name: "TodoTasks",

  components: {
    TodoTaskCard,
  },

  data() {
    return {
      newTask: "",
      todo: {},
      spinner: {
        get_todo: true,
      },
    };
  },

  created() {
    this.getTodo();
  },

  methods: {
    getTodo() {
      this.spinner.get_todo = true;
      this.$axios
        .get(`v1/todos/${this.$route.params.id}`)
        .then((response) => {
          this.todo = response.data.data;
        })
        .finally(() => {
          this.spinner.get_todo = false;
        });
    },

    addTask() {
      if (!this.newTask) {
        return;
      }

      const payload = {
        label: this.newTask,
      };

      this.$axios
        .post(`v1/todos/${this.$route.params.id}/tasks`, payload)
        .then((response) => {
          this.todo.tasks.unshift(response.data.data);
          this.newTask = "";
        });
    },

    afterDeleting(task) {
      const idx = this.todo.tasks.findIndex((o) => o.id === task.id);
      this.todo.tasks.splice(idx, 1);
    },
  },
};
</script>
