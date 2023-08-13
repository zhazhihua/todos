<script setup>
import {getCurrentInstance} from 'vue'
const props = defineProps(['id','text','isFinished','index'])
const emit = defineEmits(['check','deleteItem'])
function handleCheck (e,index) {
    emit('check',e,index)
}
function handleDeleteItem(index){
    emit('deleteItem',index)
}
</script>
<template>
  <div class="item">
    <span>
      <input
        type="checkbox"
        @change="handleCheck($event, index)"
        :checked="isFinished"
      />
      <span :class="{text:isFinished === true}">{{ text }}</span>
    </span>
    <button @click="handleDeleteItem(index)">删除</button>
  </div>
</template>
<style scoped>
.text {
    text-decoration: line-through;
}
.item::after {
  content: ".";
  display: block;
  clear: both;
  overflow: hidden;
  height: 0;
}
.item {
  width: 400px;
  margin: 10px auto;
}
.item > span {
  float: left;
}
.item button {
  float: right;
  background-color: #55bb8e;
  color: #ffffff;
  margin-left: 20px;
  outline: none;
}
</style>
