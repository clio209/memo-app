<template>
  <div id="app" class="container">
    <div class="view">
      <ul class="memo-list">
        <li>
          <MemoListItem
          v-for="(memo,index) in memos"
          :key="memo.id"
          :memo="memo"
          @titleClick="edit(index)"
          />
        </li>
        <li v-show="!memos.length">メモは一つもありません。</li>
      </ul>
      <MemoButton
        @buttonClick="memoFocus">+
      </MemoButton>
    </div>
    <div class="input">
      <form>
        <textarea class="memo-input-area" type="text" v-model="text" ref="editor"></textarea>
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

  export default {
    data()  {
      return {
      text: '',
      editIndex: null,
      memos: []
      }
    },
    props: {
      msg: String
    },
    watch: {
      memos: {
        handler: function () {
          localStorage.setItem('memos', JSON.stringify(this.memos))
        },
        deep: true
      }
    },
    mounted: function () {
      this.memos = JSON.parse(localStorage.getItem('memos')) || []
    },
    methods: {
      deleteItem: function (index) {
        index = this.editIndex
        if (confirm('削除していいですか?')) {
          this.memos.splice(index, 1)
        }
        this.text = ''
      },
      setItems: function() {
        let item = {
          id: new Date().getTime(),
          title: this.text.split(/\r\n|\r|\n/)[0],
          content: this.text,        
        }
        if (this.editIndex === null) {
          this.memos.push(item)
          this.text = ''
        } else {
          this.memos.splice(this.editIndex, 1, item)
          this.editIndex = null
        }
        this.cancel()
      },
      cancel() {
        this.text = ""
        this.editIndex = null
      },
      edit(index) {
        this.editIndex = index
        this.text = this.memos[index].content
        this.$refs.editor.focus()
      },
      memoFocus() {
        this.$refs.editor.focus()
      }
    },
    computed: {
      changeButtonText: function() {
        return this.editIndex === null ? "追加" : "編集"
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

#app li {
  line-height: 1.5;
}

#app ul {
  padding: 0;
  list-style: none;
}
</style>
