<script setup>
import { computed, onMounted, reactive} from "vue";
import { nanoid } from "nanoid";
import { ref } from "vue";
import Item from './Item.vue'
let text = ref("");
let todos = reactive([]);
function addTodo() {
  if (text.value !== "") {
    todos.unshift({ id: nanoid(), text: text.value, isFinished: false });
    text.value = "";
    localStorage.setItem("todoList", JSON.stringify(todos));
  } else {
    alert("请输入任务");
  }
}
function check(e, index) {
  todos[index].isFinished = e.target.checked;
  localStorage.setItem("todoList", JSON.stringify(todos));
}
function checkAll(e) {
  todos.forEach((item) => {
    item.isFinished = e.target.checked;
  });
}
function deleteItem(index) {
  todos.splice(index, 1);
  localStorage.setItem("todoList", JSON.stringify(todos));
}
function getData() {
  const localData = JSON.parse(localStorage.getItem("todoList"));
  if (localData !== null) {
    localData.forEach((item) => {
      todos.push(item);
    });
  }
}
onMounted(() => {
  getData();
});
</script>

<template>
  <div class="form">
    <div class="head">
      <input
        type="text"
        v-model="text"
        @keyup.enter="addTodo"
        placeholder="添加任务"
      />
      <button @click="addTodo">添加</button>
    </div>
    <div class="list">
      <template  v-for="(item,index) in todos" :key="item.id">
        <Item @check="check" @deleteItem="deleteItem" :index="index" :id="item.id" :text="item.text" :isFinished="item.isFinished" v-if="item.isFinished == false"/>
      </template>
    </div>
    <hr>
    <div class="foot">
       <template  v-for="(item,index) in todos" :key="item.id">
        <Item @check="check" @deleteItem="deleteItem" :index="index" :id="item.id" :text="item.text" :isFinished="item.isFinished" v-if="item.isFinished == true" />
      </template>
    </div>
  </div>
</template>

<style scoped>
.form {
  width: 500px;
}
.head {
  width: 500px;
}
.head input {
  height: 20px;
  width: 300px;
  padding-left: 5px;
  outline: none;
}
.head button {
  height: 40px;
  width: 80px;
  background-color: #55bb8e;
  color: #ffffff;
  margin-left: 20px;
  outline: none;
}
.foot button {
  background-color: #55bb8e;
  color: #ffffff;
  margin-left: 20px;
  outline: none;
}
</style>
