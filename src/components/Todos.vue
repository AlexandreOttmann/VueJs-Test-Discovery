<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
    <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo"/>
    </header>  
    <div class="main">
      <input type="checkbox"   id="toggle-all" class="toggle-all" v-model="allDone">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing" >
        </li>
      </ul>

   
    </div>
    <footer class="footer" v-show="hasTodo">
      <span class="todo-count">
        <strong>{{ remaining }}</strong>
        Tâche(s) restante(s)
      </span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
        <li><a href="#"  :class="{ selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
        <li><a href="#"  :class="{ selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
      </ul>
      <button class="clear-completed" @click="deleteCompleted" v-show="doneTodos">Effacer les tâches terminées</button>
      
    </footer>
  </section>
</template>

<script>

import * as Vue from 'vue'

export default {

props: {
  ParentTodos: {type: Array, default () {return []}}
},

data (){
  return {
    todos: this.ParentTodos,
    newTodo: '',
    filter: 'all',
    editing: null,
    oldTodo: ''
  }
},
watch: {
  ParentTodos(value) {
    this.todos = value
  }
},
methods: {
  addTodo() {
    this.todos.push({
      completed: false,
      name: this.newTodo
    })
    this.newTodo = ''
  },
  deleteTodo (todo) {
    this.todos = this.todos.filter( i => i !== todo)
    this.$emit('input', this.todos)
  },
  deleteCompleted () {
    this.todos = this.todos.filter(todo => !todo.completed)
    this.$emit('input', this.todos)
  },
  editTodo (todo) {
    this.editing = todo
    this.oldTodo = todo.name
  },
  doneEdit () {
    this.editing = null
  },
    cancelEdit() {
      this.editing.name = this.oldTodo
      this.doneEdit()
    }
},
computed: {
  allDone: {
    get () {
      return this.remaining === 0
    },
    set (value) {
    
        this.todos.forEach(todo => {
          todo.completed = value
        })  
    }
  },
  hasTodo () {
    return this.todos.length > 0
  },
  doneTodos(){
    return this.todos.filter(todo => todo.completed).length > 0
  },
  remaining(){
    return this.todos.filter(todo => !todo.completed).length
  },
  filteredTodos () {
    return this.todos.filter(todo => {
      if (this.filter === 'all') return true
      if (this.filter === 'todo') return !todo.completed
      if (this.filter === 'done') return todo.completed
    })
  }
},
directives:{
  focus (el, value) {
    if (value) {
      Vue.nextTick(_ => {
        el.focus()
      })
    }
  }
  
}
}

</script>

<style src="./todos.css">

</style>