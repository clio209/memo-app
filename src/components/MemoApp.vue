<template>
<div id="app" class="container">
   <div class="view">
    <ul class="memo-list">
      <li>
        <MemoListItem
         v-for="(todo, index) in todos" 
         :key="index" 
         :todo="todo"
         @increment="edit(index)"
         />
      </li>
      <li v-show="!todos.length">メモは一つもありません。</li>
      </ul>
      <MemoButton
       @buttonClick="memoFocus">+
      </MemoButton>
     </div>
  <div class="input">
       <form>
      <textarea class="memo-input-area" type="text" v-model="text" @keyup.enter="changeItems" ref="editor"></textarea>
    </form>
      <MemoButton
       @buttonClick="setItems">{{ changeButtonText }}
      </MemoButton>
      <MemoButton
       @buttonClick="deleteItem(index)">削除
      </MemoButton>
  </div>    
 
</div>
</template>

<script>
import MemoButton from './MemoButton.vue'
import MemoListItem from './MemoListItem.vue'

let nextMemoId = 1
 export default {
    data()  {
      return {
      text: '',
      editIndex: -1,
      todos: []
      }
    },
    props: {
      msg: String
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
      deleteItem: function (index) {
        index = this.editIndex
        if (confirm('削除していいですか?')) {
          this.todos.splice(index, 1)
        }
        this.text = ''
      },
      setItems: function() {
        let item = {
          id: nextMemoId++,
          title: this.text.split(/\r\n|\r|\n/)[0],
          content: this.text,        
        }
        if (this.editIndex === -1) {
          this.todos.push(item)
          this.text = ''
        } else {
          this.todos.splice(this.editIndex, 1, item)
          this.editIndex = -1
        }
        this.cancel()
      },
      cancel() {
        this.text = ""
        this.editIndex = -1
      },
      edit(index) {
        this.editIndex = index
        this.text = this.todos[index].content
        this.$refs.editor.focus()
      },
      memoFocus() {
        this.$refs.editor.focus()
      }
    },
    computed: {
      changeButtonText: function() {
        return this.editIndex === -1 ? "追加" : "編集"
      }
    },
    components: {
        MemoButton,
        MemoListItem
    }
  }
</script>

<style>
body {
  font-size: 16px;
  font-family: "Gill Sans", sans-serif;
}

.container {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  margin: auto;
}

.view {
 width:20%;
}

.input {
width: 20%;
}

.memo-input-area {
width:200px;
height: 300px;
margin: 10px;
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
  margin: 5px;
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
