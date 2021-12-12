<template>
    <div class="bg-white shadow-xl rounded-md px-4 mb-2">
      <div class="flex justify-between items-center mb-4">
        <span class="text-xs text-gray-600">Adicionado em {{overduePresent(todo.created_at)}}</span>

        <div class="flex items-center justify-center">
          <TwDropdown naked no-icon no-padding>
            <template v-slot:button-content>
              <span class="text-purple-400 hover:text-purple-500 transition">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-8 w-8"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                >
                  <path
                    d="M6 10a2 2 0 11-4 0 2 2 0 014 0zM12 10a2 2 0 11-4 0 2 2 0 014 0zM16 12a2 2 0 100-4 2 2 0 000 4z"
                  />
                </svg>
              </span>
            </template>

            <TwDropdownItem @click="onUpdate()">
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
                  d="M11 5H6C4.89543 5 4 5.89543 4 7V18C4 19.1046 4.89543 20 6 20H17C18.1046 20 19 19.1046 19 18V13M17.5858 3.58579C18.3668 2.80474 19.6332 2.80474 20.4142 3.58579C21.1953 4.36683 21.1953 5.63316 20.4142 6.41421L11.8284 15H9L9 12.1716L17.5858 3.58579Z"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              Editar
            </TwDropdownItem>

            <TwDropdownItem @click="onDelete()">
              <div class="flex items-center text-red-700 hover:text-red-800">
                <svg
                  class="mr-3 h-5 w-5"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M19 7L18.1327 19.1425C18.0579 20.1891 17.187 21 16.1378 21H7.86224C6.81296 21 5.94208 20.1891 5.86732 19.1425L5 7M10 11V17M14 11V17M15 7V4C15 3.44772 14.5523 3 14 3H10C9.44772 3 9 3.44772 9 4V7M4 7H20"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
                Excluir
              </div>
            </TwDropdownItem>
          </TwDropdown>
        </div>
      </div>

      <div class="flex justify-between items-center pb-3 gap-16">
        <RouterLink
          class="
            text-gray-900
            font-bold
            lg:text-2xl
            md:text-xl
            sm:text-lg
            truncate
          "
          :to="{ name: 'todo-tasks', params: { id: todo.id } }"
        >
          {{ todo.label }}
        </RouterLink>
   
        <span class="py-1 px-3 text-xs text-white rounded-full"
        :class="`bg-${this.status=='Pendente'? 'red' : (this.status== 'Andamento'? 'yellow': 'green')}-500`">{{ this.status }}</span>
      </div>
      <span class="-mt-10 font-semibold text-xs text-purple-800">{{`Tarefas: ${this.todo.totalTasks} - Finalizadas: ${this.todo.tasksComplete}`}}</span>
    </div> 
</template>

<script>
import TwDropdown from "@/components/Utils/TwDropdown";
import TwDropdownItem from "@/components/Utils/TwDropdownItem";


export default {
  name: "TodoCardShow",

  components: {
    TwDropdown,
    TwDropdownItem,
  },

  props: {
    todo: {
      type: Object,
      default: () => ({}),
    },
  },

  data() {
    
    return {
      status: (this.todo.totalTasks < this.todo.tasksComplete || this.todo.tasksComplete == 0 ? "Pendente" : (this.todo.tasksComplete < this.todo.totalTasks ? "Andamento" : "Finalizado")),
    };
  },
  
  methods: {
    onUpdate() {
      this.$emit("update");
    },

    onDelete() {
      this.$emit("delete");
    },

    overduePresent(dateTodo){
        if(!dateTodo){ return;}
        var data = dateTodo.split(' ').reverse()
        var x = data[1].split('-').reverse()
        var dia = x[0]
        var mes = x[1]
        var ano = x[2]

        var meses = [
            'Jan',
            'Fev',
            'Mar',
            'Abr',
            'Mai',
            'Jun',
            'Jul',
            'Ago',
            'Set',
            'Out',
            'Nov',
            'Dez'
        ]

        return dia+ ' ' + meses[mes-1] + ' ' + ano;

    }
  },
};
</script>
