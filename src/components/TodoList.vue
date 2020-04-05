<template>
  <div>
    <div class="container">
      <div class="row d-f justify-content-center">
        <div class="col-lg-12">
          <div class="card">
            <div class="card-body">
              <!--Todos list input!-->
              <h1 class="card-title">Write Your Doing List</h1>
              <div class="input-item d-f">
                <label>
                  <input
                    type="text"
                    placeholder="What is your today task?"
                    @keyup.enter="addTask"
                    v-model="newTodo"
                  />
                </label>
                <button class="btn input-btn" @click="addTask">
                  <font-awesome-icon icon="plus" />
                </button>
              </div>
              <!--Todos list Items!-->
              <div class="list-wrapper">
                <ul class="todo-list">
                  <li
                    v-for="(todo, index) in todoList"
                    :key="todo.id"
                    class="todo-item"
                  >
                    <label class="form-check">
                      <input type="checkbox" v-model="todo.completed" />
                      <span class="check-mark"></span>
                    </label>
                    <div class="todo-list-left">
                      <div
                        class="todo-list-label"
                        v-if="!todo.editing"
                        @dblclick="editTask(todo)"
                        :class="{ completed: todo.completed }"
                      >
                        {{ todo.title }}
                      </div>
                      <input
                        class="todo-list-edit"
                        type="text"
                        v-model="todo.title"
                        v-else
                        @blur="editTaskDone(todo)"
                        @keyup.enter="editTaskDone(todo)"
                      />
                    </div>
                    <div class="remove-item">
                      <button
                        class="btn input-btn trash-btn"
                        @click="removeTask(index)"
                      >
                        <font-awesome-icon icon="trash" />
                      </button>
                    </div>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      newTodo: "",
      idForTodoList: 3,
      todoList: [
        {
          id: 1,
          title: "Reading Js Doc",
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: "Watching Js Video",
          completed: false,
          editing: false
        }
      ]
    };
  },
  directives: {
    inserted(el) {
      el.focus();
    }
  },
  methods: {
    /**
     * Add new Item to Todo list
     */
    addTask() {
      if (this.newTodo.trim().length == 0) {
        return;
      }
      this.todoList.push({
        id: this.idForTodoList,
        title: this.newTodo,
        completed: false
      });
      this.newTodo = "";
      this.idForTodoList++;
    },
    /**
     * Remove Related Task
     * @param index
     */
    removeTask(index) {
      this.todoList.splice(index, 1);
    },
    /**
     *  Edit Task Item
     * @param todo
     */
    editTask(todo) {
      todo.editing = true;
    },
    /**
     * Edit Done
     * @param todo
     */
    editTaskDone(todo) {
      todo.editing = false;
    }
  }
};
</script>
