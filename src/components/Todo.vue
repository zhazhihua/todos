<script setup>
import { computed, onMounted, reactive } from "vue";
import { nanoid } from "nanoid";
import { ref } from "vue";
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
}
function checkAll(e) {
  todos.forEach((item) => {
    item.isFinished = e.target.checked;
  });
  console.log(input.value);
}
function deleteChecked() {
  const newArr = todos.filter((item) => {
    return item.isFinished === false;
  });
  todos = newArr;
  location.reload();
  localStorage.setItem("todoList", JSON.stringify(todos));
}
function deleteItem(index) {
  console.log(index);
  todos.splice(index, 1);
  localStorage.setItem("todoList", JSON.stringify(todos));
}
function getData() {
  const localData = JSON.parse(localStorage.getItem("todoList"));
  if (localData !== null) {
    localData.forEach((item) => {
      item.isFinished = false;
      todos.push(item);
    });
  }
  console.log(localData);
}
onMounted(() => {
  getData();
});
const checkNum = computed(() => {
  let count = 0;
  todos.forEach((item) => {
    if (item.isFinished === true) {
      count++;
    }
  });
  return count;
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
      <div class="item" v-for="(item, index) in todos" :key="item.id">
        <span>
          <input
            type="checkbox"
            @change="check($event, index)"
            :checked="item.isFinished"
          />
          <span>{{ item.text }}</span>
        </span>
        <button @click="deleteItem(index)">删除</button>
      </div>
    </div>
    <div class="foot">
      <span>
        <input
          type="checkbox"
          @change="checkAll($event)"
          :checked="checkNum === todos.length && todos.length !== 0"
        />全选&nbsp;已选择{{ checkNum }}/总计{{ todos.length }}
      </span>
      <button @click="deleteChecked">删除已勾选</button>
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
.list .item::after {
  content: ".";
  display: block;
  clear: both;
  overflow: hidden;
  height: 0;
}
.list .item {
  width: 400px;
  margin: 10px auto;
}
.list .item > span {
  float: left;
}
.list .item button {
  float: right;
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
