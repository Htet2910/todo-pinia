<template>
  <div class="todo-app">
    <h1>To Do List</h1>
    <form @submit.prevent="addItemAndClear(todo)">
      <input v-model="todo" type="text" /><button>Add</button>
       <div class="alert-div">
         <div v-show="store.showAlert" class="alert">
           Please type something...
         </div>
       </div>
    </form>
     <div v-for="todo in todoList" :key="todo.id" class="list">
    <div class="item">
      <span :class="{ completed: todo.completed }">{{ todo.item }}</span>
      <div>
        <span @click.stop="toggleCompleted(todo.id)">&#10004;</span>
        <span @click="deleteTodo(todo.id)" class="x">&#10060;</span>
      </div>
    </div>
  </div>
  </div>

</template>

<script>
import { ref } from "vue";
import { useTodoListStore } from "../store/useTodoListStore ";
import { storeToRefs } from "pinia";

export default {
    // components: { TodoList },
    setup() {
        const todo = ref("");
        const store = useTodoListStore();
        function addItemAndClear(item) {
            if (item.length === 0) {
                store.inputAlert();
                return;
            }
            store.addTodo(item);
            // console.log(store.$state);
            todo.value = "";
        }
         const { todoList } = storeToRefs(store);
        const { toggleCompleted, deleteTodo } = store;
        return { todoList, toggleCompleted, deleteTodo,todo, addItemAndClear, store };
    },
};
</script>
<style scoped>
.todo-app {
  padding: 5vh;
  min-height: 100vh;
  width: 30vw;
  background-color: #f7f9fc;

}
span {
  margin: 0 10px;
  cursor: pointer;
}

.completed {
  text-decoration: line-through;
}

.list {
  display: flex;
  justify-content: center;
}

.item {
  display: flex;
  font-size: 1.5em;
  justify-content: space-between;
  width: 80vw;
  padding: 5px;
}
form {
  margin-bottom: 15px;
}

input {
  margin-top: 25px;
  margin-bottom: 15px;
  height: 20px;
  width: 50%;
}
button {
  margin-left: 30px;
  background-color: #2f6089;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  font-weight: 800;
  color: white;
  width: 15%;
}

.alert-div {
  min-height: 25px;
}

.alert {
  color: #d1495b;
  font-size: 1em;
  font-weight: 600;
}
</style>
