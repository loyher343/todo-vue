<template>
    <div>
        <input type="text" class="todo-input" placeholder="what needs to be done"
        v-model="newTodo" @keyup.enter="addTodo">
        <div v-for="todo in todos" :key="todo.id" class="todo-item">
            {{todo.title}}
        </div>
        
    </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      todos: [
          {
            'id': 1,
            'title': 'Finish Vue Screencast',
            'completed': false
          },
          {
            'id': 2,
            'title': 'Take over World',
            'completed': false
          }
      ]
    }
  },
  methods: {
      addTodo() {
        //   alert('adding')
        if(this.newTodo.trim().length == 0){
            return
        }

        //'this' references 'data' property
        this.todos.push({
            id: this.idForTodo,
            title: this.newTodo,
            completed: false
        })

        this.newTodo = ''
        this.idForTodo++
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.todo-input{
    width: 100%;
    padding: 10px 18px;
    font-size: 16px;
    margin-bottom: 16px;
}
</style>
