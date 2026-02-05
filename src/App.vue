<template>
  <div>
    <!-- Formulaire -->
    <h2>Todo List</h2>
    <form @submit.prevent="addTodo">
      <input type="text" placeholder="Add a new task" v-model="todoTitle" />
      <button>Add</button>
    </form>

    <!-- Liste -->
    <div class="todos">
      <p v-if="!todos.length">Add a new task !</p>
      <ul v-else>
        <li v-for="todo in todoSorted" :key="todo.id">
          <label :for="todo.id" :class="{ done: todo.completed }">
            <input type="checkbox" :id="todo.id" v-model="todo.completed" />
            {{ todo.title }}
          </label>
        </li>
      </ul>
    </div>

    <hr />
    <div v-if="existCompletedTask">
      <input type="checkbox" id="hide" v-model="hideCompleted" />
      <label for="hide"> Hide completed tasks </label>
    </div>
  </div>
</template>

<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import { computed, reactive, ref } from "vue";

type TodoType = {
  title: string;
  completed: boolean;
  id: string;
};

const todos = reactive<TodoType[]>([
  {
    title: "Update my todo",
    completed: false,
    id: uuidv4(),
  },
  {
    title: "Meeting with John Doe",
    completed: true,
    id: uuidv4(),
  },
  {
    title: "Yodaga",
    completed: true,
    id: uuidv4(),
  },
]);

// todo model
const todoTitle = ref("");

//add todo
const addTodo = () => {
  if (todoTitle.value) {
    todos.push({
      title: todoTitle.value,
      completed: false,
      id: uuidv4(),
    });
    todoTitle.value = "";
  }
};

//hide edone task
const hideCompleted = ref(false);

//display first task todo
const todoSorted = computed(() => {
  //sort
  let result = [...todos].sort(
    (a, b) => Number(a.completed) - Number(b.completed),
  );

  //if hide completed checked
  if (hideCompleted.value) {
    return result.filter((todo) => !todo.completed);
  }
  return result;
});

const existCompletedTask = todoSorted.value.filter(
  (todo) => !todo.completed,
).length;
</script>

<style scoped>
.todos {
  margin-top: 15px;
}
ul {
  padding-left: 0;
}
ul > li {
  list-style: none;
}

.done {
  text-decoration: line-through;
}
</style>
