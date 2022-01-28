<template>
<div>
  <MemoForm
   v-model="newItem"
   placeholder="New memo"
   />
</div>

<ul class="memo-list">
<li> 
  <MemoListItem
   v-for="todo in todos" 
   :key="todo" 
   :todo="todo"
   @fuga="editTodo"
   @piyo="deleteItem"
   @increment="editTodo"
  />
</li>
<li v-show="!todos.length">Todoは一つもありません。</li>
</ul>

<div>
<MemoButton
@click="addItem"
v-model="newItem"
>
  追加
</MemoButton>
</div>
</template>

<script>
import MemoButton from './MemoButton.vue'
import MemoForm from './MemoForm.vue'
import MemoListItem from './MemoListItem.vue'

let nextMemoId = 1

export default {
    data()  {
      return {
      newItem: '',
      editedTodo: null,
      beforeEditCache: '',
      todos: []
      }
    },  
    watch: {
      todos: {
        handler: function () {
          localStorage.setItem('todos', JSON.stringify(this.todos))
        },
        deep: true
      }
    },
    mounted: function () {
      this.todos = JSON.parse(localStorage.getItem('todos')) || []
    },
methods: {
      addItem: function () {
        const item = {
          id: nextMemoId++,
          title: this.newItem.split(/\r\n|\r|\n/)[0],
          content: this.newItem,
        }
        this.todos.push(item)
        this.newItem = ''
      },
      deleteItem: function (todo) {
        if (confirm('削除していいですか?')) {
          this.todos.splice(this.todos.indexOf(todo), 1)
        }
      },
      editTodo: function (todo) {
        this.beforeEditCache = todo.content
        this.editedTodo = todo
        this.newItem = todo
      },
      doneEdit: function (todo) {
        if (!this.editedTodo) {
          return
        }
        this.editedTodo = null
        const title = todo.title.trim()
        if (title) {
          todo.title = title
        }
      },
      cancelEdit: function (todo) {
        this.editedTodo = null
        todo.title = this.beforeEditCache
      }
    },
    computed: {
      remaining: function () {
        return this.todos.filter(function (todo) {
          return !todo.isDone
        })
      }
    },
    directives: {
      'todo-focus' (element, binding) {
        if (binding.value) {
          element.focus()
        }
      }
    },
components: {
  MemoListItem,MemoForm,MemoButton
}
}
</script>

<style>
body {
  font-size: 16px;
  font-family: "Gill Sans", sans-serif;
}

.container {
  width: 300px;
  margin: auto;
}

#app h1 {
  font-size: 16px;
  border-bottom: 1px solid #ddd;
  padding: 16px 0;
}

#app li {
  line-height: 1.5;
}

#app input[type="text"] {
  padding: 2px;
}

.command {
  font-size: 12px;
  cursor: pointer;
  color: #08c;
}

#app ul {
  padding: 0;
  list-style: none;
}

#app .view > lavel.done {
  text-decoration: line-through;
  color: #bbb;
}

.info {
  color: #bbb;
  font-size: 12px;
  font-weight: normal;
}
#app h1 > button {
  float: right;
}


.todo-list li.editing .edit {
	display: block;
}

.todo-list li.editing .view {
	display: none;
}

.todo-list li .edit {
	display: none;
}

</style>