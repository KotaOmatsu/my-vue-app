<template>
  <div>
    <div class="app-title"></div>
    <div class="flex">
      <div class="todolist big">
        <div class="ttl">Todo List</div>

        <div>
          <div class="input_text">
            <input type="text" v-model="inputTodo" @keydown.enter="addTodo" />
            <div v-on:click="addTodo" class="red_btn">add</div>
          </div>
        </div>

        <div>
          <draggable
            v-model="todos"
            group="myGroup"
            :options="options"
            @start="drag = true"
            @end="drag = false"
          >
            <div v-for="(todo, index) in todos" v-bind:key="index" class="todo">
              <!-- <div class="todo__checkbox">
          <input type="checkbox" v-model="todo.isDone" />
        </div>

        <div v-if="todo.isDone" class="todo__text todo__text--done">
          {{ index }}:{{ todo.text }}
        </div>
        <div v-else class="todo__text">{{ index }}:{{ todo.text }}</div> -->
              <div class="list">
                <div>{{ index + 1 }}．{{ todo.text }}</div>
              </div>

              <div class="btns">
                <div v-on:click="completeTodo(index)" class="btn">✅</div>
                <div v-on:click="deleteTodo(index)" class="todo__delete">🗑</div>
              </div>
            </div>
          </draggable>
        </div>
      </div>

      <div class="completelist big">
        <div class="ttl">Complete✅</div>
        <div v-on:click="kanzenCompleteTodo" class="red_btn">clear</div>
        <draggable
          v-model="completetodos"
          group="myGroup"
          :options="options"
          @start="drag = true"
          @end="drag = false"
        >
          <div
            v-for="(completetodo, index) in completetodos"
            v-bind:key="index"
            class="todo"
          >
            <div class="list">{{ completetodo.text }}</div>
            <div v-on:click="fukugenTodoc(index)" class="btn">🔄</div>
          </div>
        </draggable>
      </div>

      <div class="deletelist big">
        <div class="ttl">deleted🗑</div>
        <div v-on:click="AllkanzendeleteTodo" class="red_btn">clear</div>

        <draggable
          v-model="deletetodos"
          group="myGroup"
          :options="options"
          @start="drag = true"
          @end="drag = false"
        >
          <div
            v-for="(deletetodo, index) in deletetodos"
            v-bind:key="index"
            class="todo"
          >
            <div class="list">{{ deletetodo.text }}</div>
            <div class="btns">
              <div v-on:click="fukugenTodo(index)" class="btn">🔄</div>
              <div v-on:click="kanzendeleteTodo(index)" class="todo__delete">
                ❌
              </div>
            </div>
          </div>
        </draggable>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable"

export default {
  name: "dnd",

  components: { draggable },

  data() {
    return {
      options: {
        group: "myGroup",
        animation: 200,
      },
      inputTodo: "",
      todos: [
        {
          text: "example1",
          isDone: false,
        },
        {
          text: "example2",
          isDone: false,
        },
        {
          text: "example3",
          isDone: false,
        },
      ],
      completetodos: [
        {
          text: "example4",
          isDone: true,
        },
      ],
      deletetodos: [
        {
          text: "example5",
          isDone: true,
        },
      ],
    }
  },
  methods: {
    addTodo(event) {
      if (event.keyCode !== 13) {
        if (this.inputTodo !== "") {
          const todo = { text: this.inputTodo, isDone: false }
          this.todos.unshift(todo)
          this.inputTodo = ""
        }
      }
      if (this.inputTodo !== "") {
        const todo = { text: this.inputTodo, isDone: false }
        this.todos.unshift(todo)
        this.inputTodo = ""
      }
    },
    completeTodo(index) {
      const completetodo = { text: this.todos[index].text, isDone: true }
      this.completetodos.unshift(completetodo)
      this.todos.splice(index, 1)
      window.setInterval(this.kanzenCompleteTodo, 3600000)
    },
    kanzenCompleteTodo(index) {
      const result = window.confirm("全ての項目を完全に削除しますか？")
      if (result) {
        this.completetodos.splice(index, this.completetodos.length)
      }
    },
    deleteTodo(index) {
      const deletetodo = { text: this.todos[index].text, isDone: true }
      this.deletetodos.unshift(deletetodo)
      this.todos.splice(index, 1)
    },
    fukugenTodo(index) {
      const todo = { text: this.deletetodos[index].text, isDone: false }
      this.todos.unshift(todo)
      this.deletetodos.splice(index, 1)
    },
    fukugenTodoc(index) {
      const todo = { text: this.completetodos[index].text, isDone: false }
      this.todos.unshift(todo)
      this.completetodos.splice(index, 1)
    },
    kanzendeleteTodo(index) {
      const result = window.confirm("この項目を完全に削除しますか？")
      if (result) {
        this.deletetodos.splice(index, 1)
      }
    },
    AllkanzendeleteTodo(index) {
      const result = window.confirm("全ての項目を完全に削除しますか？")
      if (result) {
        this.deletetodos.splice(index, this.deletetodos.length)
      }
    },
  },
}
</script>

<style scoped>
@import url("https://unpkg.com/ress/dist/ress.min.css");

.input_text {
  display: flex;
}
input {
  width: 85%;
  border: 3px solid rgb(102, 203, 221);
  background-color: #fff;
  margin: 0.2rem;
}
.add_btn {
  margin-left: 0.25rem;
  font-size: 20px;
  width: 5%;
  cursor: pointer;
}
.flex {
  display: flex;
  justify-content: center;
  width: 100%;
  min-height: 100vh;
  background: rgb(180, 240, 255);
  user-select: none;
}

.ttl {
  font-weight: 100000;
  font-size: 1.5rem;
  text-align: center;
  color: #fff;
  background: #1a3457;
  border-radius: 10px;
}
.todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  border-radius: 10px;
  background: #fff;
  /* border: 3px solid rgb(102, 203, 221); */
  box-shadow: 5px 5px 2px rgb(105, 105, 105);
  margin: 0.5rem 0;
}
.todo:nth-child(2n) {
  background: #e7e7e7;
}
.todo:hover {
  /* color: white; */
  background-color: #dee8fd;
  transition: ease 0.3s;
}
.big {
  background: #d6f6ff;
  border-radius: 40px;
  margin: 1rem;
}
.todolist {
  padding: 1.4rem;
  display: flex;
  flex-direction: column;
  width: 30%;
  /* min-width: 400px; */
}

.completelist {
  padding: 1.4rem;
  width: 30%;
  /* min-width: 300px; */
}
.deletelist {
  padding: 1.4rem;
  width: 30%;
  /* min-width: 300px; */
}

.todo__text {
  margin-left: 2rem;
  text-align: left;
}

.todo__delete {
  margin-left: 2rem;
  margin-right: 0.5rem;
  cursor: pointer;
}
.todo__delete:hover {
  text-shadow: 0 0 5px red;
}
.btns {
  display: flex;
}
.btn {
  cursor: pointer;
  user-select: none;
}
.btn:hover {
  text-shadow: 0 0 5px #fff;
}
.red_btn {
  cursor: pointer;
  font-weight: bold;
  color: #fff;
  text-align: center;
  background: red;
  border-radius: 10px;
  box-shadow: 2px 2px 0 rgb(170, 1, 1);
  padding: 2px;
  margin: 5px;
  font-weight: 100000;
}
.list {
  display: flex;
  flex-direction: column;
}

@media screen and (max-width: 860px) {
  .flex {
    flex-direction: column;
  }
  .todolist,
  .completelist,
  .deletelist {
    width: 100%;
    max-height: 40vh;
    overflow-y: scroll;
  }
}
</style>
