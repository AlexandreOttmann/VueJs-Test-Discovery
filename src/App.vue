<template>
  <div>
    <todos :ParentTodos="todos"></todos> 
    <todos :ParentTodos="todos"></todos> 
    <button  @click="addTodo">Ajouter une tâche Jean</button>
  </div>
</template>    
    
<script>

 import Todos from './components/Todos.vue'
    
 export default {
  data () {
    return {
      todos: [{
        name: 'Démo',
        completed: true
      }]
    }
  },
  methods:{
    addTodo (){
      this.todos.push({
        name: 'jean',
        completed: false
      })
    }
  },
      components: {
        Todos
      }
    }
    
</script>


