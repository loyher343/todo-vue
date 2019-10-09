<template>
    <div>
        <input type="text" class="todo-input" placeholder="what needs to be done"
        v-model="newTodo" @keyup.enter="addTodo">
        <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
          <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed">
            <div class="todo-item-label" :class="{ completed : todo.completed }" v-if="!todo.editing" @dblclick="editTodo(todo)">{{todo.title}}</div>
            <input class="todo-item-edit" v-else type="text" v-model="todo.title" 
            @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
          </div>  
          <div class="remove-item" @click="removeTodo(index)">
            &times;
          </div>
        </div>
        <div class="extra-container">
          <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
          <div>{{ remaining }} items left</div>
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
      beforeEditCache: '',
      todos: [
          {
            'id': 1,
            'title': 'Finish Vue Screencast',
            'completed': false,
            'editing': false
          },
          {
            'id': 2,
            'title': 'Take over World',
            'completed': false,
            'editing': false
          }
      ]
    }
  },
  computed: {         //composing new data, derived from other data. should not mutate data or not accept params and always return something
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
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
      },
      removeTodo(index) {
        this.todos.splice(index, 1)
      },
      editTodo(todo) {
        this.beforeEditCache = todo.title
        todo.editing = true
      },
      doneEdit(todo){
        if(todo.title.trim().length == ''){
          todo.title = this.beforeEditCache
        }
        todo.editing = false
      },
      cancelEdit(todo){
        todo.title = this.beforeEditCache
        todo.editing = false
      },
      checkAllTodos() { 
        this.todos.forEach( (todo) => todo.completed = event.target.checked)
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

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-item:hover {
  cursor: pointer;
  margin-left: 14px;
  color: black;
  
}

.todo-item-left{
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}

button {
  font-size: 14px;
  background-color: white;
  appearance: none;

  
}

button:hover {
  background-color: lightgreen;
}

button:focus {
  outline: none;
}




</style>

