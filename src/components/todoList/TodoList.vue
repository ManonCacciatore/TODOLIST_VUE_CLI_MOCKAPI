<script setup>

  import { reactive , onMounted } from "vue";
  import DB from "@/services/DB";
  import TodoListAddForm from './TodoListAddForm.vue';
  import TodoListFooter from './TodoListFooter.vue';
  import Todo from './Todo.vue';


  const todos = reactive([]);

  onMounted(async () => {
    DB.setApiURL("https://691b2f262d8d78557571e7d5.mockapi.io/")
    todos.splice(todos.length, 0, ...(await DB.findAll())) ;
  });


</script>
<template>
    <!-- CARD LISTE -->
    <section class="bg-slate-100 rounded-xl shadow ring-1 ring-slate-200/60 overflow-hidden"
      aria-labelledby="todo-heading">
      <h2 id="todo-heading" class="sr-only">Todo list</h2>

      <TodoListAddForm />


      <!-- LISTE DES TODOS -->
      <ul class="m-4 divide-y divide-slate-200" role="list" aria-label="Todos">
        <!-- ITEM (exemple) -->
        <todo v-for="todo in todos" :key="todo.id" :todo="todo"/>
      </ul>

      <!-- FOOTER DE LISTE -->
       <TodoListFooter />

    </section>
</template>
<style scoped></style>