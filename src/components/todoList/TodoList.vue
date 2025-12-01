<script setup>

  import { reactive , onMounted } from "vue";
  import DB from "@/services/DB";
  import TodoListAddForm from './TodoListAddForm.vue';
  import TodoListFooter from './TodoListFooter.vue';
  import Todo from './Todo.vue';

  const props = defineProps({
    apiURL : {type: String, required: true}
  });


  const todos = reactive([]);

  onMounted(async () => {
    DB.setApiURL(props.apiURL)
    todos.splice(todos.length, 0, ...(await DB.findAll())) ;
  });

  // FONCTIONS CRUD

  const createItem = async (content) => {
   const todo = await DB.create(content);
   todos.push(todo);
  }

  const deleteOneById = async (id) => {
    await DB.deleteOneById(id)
    todos.splice(todos.findIndex((todo) => todo.id === id), 1)
  }


</script>
<template>
    <!-- CARD LISTE -->
    <section class="bg-slate-100 rounded-xl shadow ring-1 ring-slate-200/60 overflow-hidden"
      aria-labelledby="todo-heading">
      <h2 id="todo-heading" class="sr-only">Todo list</h2>

      <TodoListAddForm @on-submit-add-form="createItem($event)" />


      <!-- LISTE DES TODOS -->
      <ul class="m-4 divide-y divide-slate-200" role="list" aria-label="Todos">
        <!-- ITEM (exemple) -->
        <todo v-for="todo in todos" :key="todo.id" :todo="todo" @on-delete="deleteOneById($event)"/>
      </ul>

      <!-- FOOTER DE LISTE -->
       <TodoListFooter />

    </section>
</template>
<style scoped></style>