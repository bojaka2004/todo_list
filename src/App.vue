<template>
  <div id="app">
    <div class="container">
      <h1 class="title">Todo List</h1>
      <ul>
        <li v-for="todo in todos" :key="todo.id" :class="{ done: todo.done }">
          <span>{{ todo.text }}</span>
          <div>
            <button @click="doneTodo(todo.id)">{{ todo.done ? 'Undone' : 'Done' }}</button>
            <button @click="removeTodo(todo.id)">Remove</button>
          </div>
        </li>
      </ul>
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new task" />
    </div>
  </div>

</template>

<script setup>
import { ref } from 'vue';

const todos = ref([
  { id: 2, text: '精通C++', done: false },
  { id: 3, text: '精通Python', done: true },
  { id: 1, text: '精通VUE', done: true }
]);
const newTodo = ref('');

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      done: false
    });
    newTodo.value = '';
  }

  // 排序 未完成的在前面
  todos.value.sort((a, b) => a.done - b.done);
};

const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id);
};

const doneTodo = (id) => {
  todos.value = todos.value.map(todo => {
    if (todo.id === id) {
      todo.done = !todo.done;
    }
    return todo;
  });
};
</script>

<style lang="less" scoped>
#app {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f8ebe6;
  background-image: url(./assets/texture.png);


  .container {
    width: 400px;
    height: 700px;
    border-radius: 5px;
    overflow: hidden;
    background-color: rgb(250, 211, 138);
    position: relative;
    padding-bottom: 40px;

    .title {
      width: 100%;
      font-size: 26px;
      padding: 5px 10px;
      background-color: #fa5d19;
      background-image: url(./assets/texture.png);
      color: #fff;
    }

    input {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 40px;
      border: none;
      outline: none;
      padding: 0 10px;
      font-size: 1.2rem;
      box-sizing: border-box;
    }


    ul {
      height: calc(100% - 45px);
      overflow: auto;

      // 美化滚动条
      scrollbar-width: thin;

      li {
        min-height: 50px;
        list-style: none;
        width: 100%;

        &:nth-child(odd) {
          background-color: #edc3ae;

          div {
            button:hover {
              background-color: #f8ebe6;
              color: #363433;
            }
          }

        }

        &:nth-child(even) {
          background-color: #f8ebe6;

          div {
            button:hover {
              background-color: #edc3ae;
              color: #fff;
            }
          }

        }

        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 5px;


        span {
          color: #15231b;
          font-size: 1.2rem;
          font-weight: 600;
          margin-right: 10px;
          word-break: break-all;
        }

        div {
          min-width: 150px !important;

          button {
            float: right;
            margin-right: 10px;
            padding: 5px 10px;
            border: none;
            background-color: #fa5d19;
            background-image: url(./assets/texture.png);
            color: #fff;
            cursor: pointer;
            transition: all 0.1s;

            &:first-child {
              margin-right: 0;
            }
          }
        }

      }


      .done {
        background-color: #999694 !important;
        text-decoration: line-through;
      }
    }
  }
}
</style>