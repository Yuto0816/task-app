<template>
    <h1>{{ message }}</h1>
    <p>count is: {{ counter.count }}</p>
    <v-btn @click="increment">count is: {{ counter.count }}</v-btn>
    <!-- v-bind,v-on -->
    <input :value="text" @input="onInput" placeholder="リファクタ前">
    <p>{{ text }}</p>
    <!-- v-model -->
    <input v-model="texts" placeholder="リファクタ後">
    <p>{{ texts }}</p>
    <!-- 条件付きレンンダリング -->
    <v-btn @click="toggle">toggle</v-btn>
    <h3 v-if="awesome">Vue is awesome!</h3>
    <h3 v-else>Oh no!</h3>
    <!-- ToDoボタン -->
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" required placeholder="new todo">
      <v-btn type="submit">Add Todo</v-btn>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.text }}
        <button @click="removeTodo(todo)">X</button>
      </li>
    </ul>
  </template>
  <script setup>
  import { ref,reactive } from 'vue'
  // import { reactive } from 'vue'
  
  // reactive（オブジェクトに対して動作する）
  const counter = reactive({
    count: 0
  })
  console.log(counter.count) 
  
  // ref（任意の値を読み取り、「.value」で内部の値を表示させる）
  const message = ref('Hello World!')
  console.log(message.value) 
  
  // ボタン
  function increment() {
    counter.count++
  }
  // バインディング
  const text = ref('かきくけこ')
  function onInput(evt){
    console.log('evt',evt)
    // targetはイベントが発生した要素を指定
    text.value = evt.target.value
    console.log('evt.target.value',evt.target.value)
  }
  const texts = ref('あいうえお')
  
  // toggle
  const awesome = ref(true)
  function toggle(){
    awesome.value = !awesome.value
  }
  
  // ToDoボタン
  let id = 0
  
  const newTodo = ref('')
  const todos = ref([
    { id: id++, text: 'Learn HTML' },
    { id: id++, text: 'Learn JavaScript' },
    { id: id++, text: 'Learn Vue' }
  ])
  
  function addTodo() {
    todos.value.push({ id: id++, text: newTodo.value })
    newTodo.value = ''
  }
  
  function removeTodo(todo) {
    todos.value = todos.value.filter((t) => t !== todo)
  }
  </script>