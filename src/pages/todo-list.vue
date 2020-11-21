<template>
  <div>
    <input type="text" class="todo-input" placeholder="¿What you wanna do?" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <q-checkbox  v-model="todo.completed"></q-checkbox>
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" >
       <q-icon name="ti-close" @click="dialog = true"/>
      </div>
    </div>
    </transition-group>

    <div class="extra-container">
      <div><label><input class="check-all-item q-gutter" type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label></div>
      <div>{{ remaining }} items left</div>
    </div>

    <div class="extra-container">
      <div>
        <q-btn class="bg-purple-3" :class="{ active: filter == 'all' }" @click="filter = 'all'">All</q-btn>
        <q-btn class="bg-purple-3" :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</q-btn>
        <q-btn class="bg-purple-3" :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</q-btn>
      </div>

      <div>
        <transition name="fade">
        <q-btn class="bg-purple-3" v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</q-btn>
        </transition>
      </div>
     
  <div class="q-pa-md">
    <q-dialog v-model="dialog" persistent>
      <q-card>
        <q-card-section class="row items-center bg-deep-purple-3">
          <span class="q-ml-sm">¿Are you sure to delete this task?</span>
        </q-card-section>


        <!-- Notice v-close-popup -->
        <q-card-actions class="bg-deep-purple-3" align="right">
          <q-btn flat label="Cancel" color="white" v-close-popup="cancelEnabled" :disable="!cancelEnabled" />
          <q-btn flat label="Delete" @click="removeTodo(index)" color="white" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>

     
  </div>

    </div>
  
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      dialog: false,
      cancelEnabled: true,
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': 'Wake up early',
          'completed': false,
          'editing': false,
        },
        {
          'id': 2,
          'title': 'Take over world',
          'completed': false,
          'editing': false,
        },
      ]

    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
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
      if (this.newTodo.trim().length == 0) {
        return
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      })
      this.newTodo = ''
      this.idForTodo++
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title
      todo.editing = true
    },
    doneEdit(todo) {
      if (todo.title.trim() == '') {
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },
    removeTodo(index) {
       this.todos.splice(index, 1)
      
    },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)

    }
  }
}
</script>

<style lang="scss">
  
  @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

  .todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    &:focus {
      outline: 0;
    }
  }
  .todo-item {
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
    font-size: 18px;
    border-top: 1px solid lightgrey;
  }
  .remove-item {
    cursor: pointer;
    position: absolute;
    right: 10px;
    color: grey;
    margin-left: 14px;
    &:hover {
      color: red;
      size: 20px;
      font-size: 1.3em;
      transition: .3s;
     
    }
  }
 
  .todo-item-left {
    position: relative;
    left: 10px;
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }
  .todo-item-edit {
    margin-top: 10px;
    font-size: 24px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc; //override defaults
    font-family: 'Roboto','Avenir', Helvetica, Arial, sans-serif;
    &:focus {
      outline: none;
    }
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 18px;
    border-top: 1px solid lightgrey;
    padding: 10px 10px;

    
  }
  button {
    font-size: 14px;
    appearance: none;
    margin:2px;
    &:hover {
      background: rgba(105, 30, 226, 0.664);
    }
    &:focus {
      outline: none;
    }
  }
  .bg-brand{
    background: #9247f5;
  }

  // CSS Transitions
  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }

</style>