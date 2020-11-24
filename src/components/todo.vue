<template>
  <div id="frame">
    <div class="todo-box box">
      <h1>TODO</h1>
      <div class="input-box">
        <div class="input-type">
          <input
            class="text-box text"
            type="text"
            v-model="type"
            v-on:keypress.enter="todoList"
          />
          <input class="text-box click" type="button" v-on:click="todoList" />
        </div>
      </div>
      <div class="origin">
        <draggable group="people">
          <list
            class="list-group-item"
            v-for="todo in todos"
            v-bind:key="todo.id"
            v-bind:value="todo"
            v-on:delete="deleteTodo"
            v-on:ended="endComplete"
          />
        </draggable>
      </div>
    </div>

    <div class="complete-box box">
      <h1>COMPLETE</h1>
      <draggable group="people">
        <list
          class="list-group-item"
          v-for="complete in completes"
          v-bind:key="complete.id"
          v-bind:value="complete"
          v-on:delete="deleteComplete"
        />
      </draggable>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import draggable from "vuedraggable";
import list from "./list";
import logger from "js-logger";

Vue.component("list", list);
Vue.use(draggable);
logger.useDefaults();

export default {
  components: {
    draggable,
  },
  data: () => ({
    todos: [],
    completes: [],
    id: 0,
    type: "",
    isHidden: true,
  }), //틀의 초기값,기본값
  methods: {
    //사용할 함수들
    todoList: function () {
      const addList = {
        id: this.id++,
        text: this.type,
        isHidden: this.isHidden,
      };
      this.todos.push(addList);
      this.type = "";
      logger.info("todoList", this.todos);
    }, // id값과 type의 값을 (addList를) todos에 문자열로 하나씩 추가시킨다
    deleteTodo: function (id) {
      this.todos.splice(
        this.todos.findIndex((todo) => todo.id === id),
        1
      );
      logger.info("deleteTodo");
    },
    deleteComplete: function (id) {
      this.completes.splice(
        this.completes.findIndex((complete) => complete.id === id),
        1
      );
      logger.info("delete com");
    },
    endComplete: function (id) {
      alert(id);
      const index = this.todos.findIndex((todo) => todo.id === id); // 변수에 넣어서 사용
      // end버튼 있다가 없애는것
      this.todos[index].isHidden = !this.todos[index].isHidden; //complete에 추가될때는 isHidden(true)을 부정하는 !isHidden(false)를 isHidden에 넣어서 내보내겠다
      this.completes.push(this.todos[index]); //todos에 있는 인덱스 값들을 찾아서 completes에 추가하겠다 // index값의 todos의 데이터 통째로 push하겠다
      this.todos.splice(index, 1); // 그 후에 todos에서 삭제하겠다 //index(시작하는위치)에서 한개를 삭제
    },
  },
};
</script>

<style>
.todo-box {
  float: left;
  width: 50%;
  height: 100em;
  background-color: rgba(65, 64, 95, 0.13);
}
.complete-box {
  float: left;
  width: 50%;
  height: 100em;
  background-color: rgba(65, 64, 95, 0.13);
}
h1 {
  padding-top: 3%;
  text-align: center;
  font-family: "Lora", serif;
}

.input-box {
  background-color: rgba(255, 255, 255, 0.5);
  width: 50%;
  height: 3em;
  margin: 0 auto;
  overflow: hidden;
  border-radius: 2em;
  margin-top: 4%;
}

.input-type {
  width: 60%;
  margin: 0 auto;
  overflow: hidden;
  padding-top: 2%;
}
.text-box {
  display: block;
  border: none;
  float: left;
}
.text {
  width: 80%;
  height: 1.5em;
  background-color: rgba(112, 77, 156, 0.116);
}
.click {
  width: 18%;
  height: 1.6em;
  background-color: rgba(79, 32, 117, 0.534);
}
</style>
