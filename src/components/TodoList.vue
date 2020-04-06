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

              <!--Todos information!-->
              <div class="todo-information">
                <div>
                  <label>
                    <input
                      type="checkbox"
                      :checked="!anyRemaining"
                      @click="changeAllTodos"
                    />
                    <span class="check-mark"></span>
                    Check All
                  </label>
                </div>
                <div class="remaining">{{ remaining }} items left</div>
              </div>

              <!-- Todos information status!-->
              <div class="todo-information">
                <div>
                  <button
                    :class="{ active: filter === 'all' }"
                    @click="filter = 'all'"
                  >
                    All
                  </button>
                  <button
                    :class="{ active: filter === 'active' }"
                    @click="filter = 'active'"
                  >
                    Active
                  </button>
                  <button
                    :class="{ active: filter === 'completed' }"
                    @click="filter = 'completed'"
                  >
                    Completed
                  </button>
                </div>

                <div>
                  <transition name="fade">
                    <button v-if="showCompletedButton" @click="clearCompleted">
                      Clear Completed
                    </button>
                  </transition>
                </div>
              </div>

              <!--Todos list Items!-->
              <div class="list-wrapper">
                <ul class="todo-list">
                  <li
                    v-for="(todo, index) in todoListFiltered"
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
      filter: "all",
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

  computed: {
    remaining() {
      return this.todoList.filter(todo => !todo.completed).length;
    },

    anyRemaining() {
      return this.remaining !== 0;
    },

    todoListFiltered() {
      if (this.filter === "all") {
        return this.todoList;
      } else if (this.filter === "active") {
        return this.todoList.filter(todo => !todo.completed);
      } else if (this.filter === "completed") {
        return this.todoList.filter(todo => todo.completed);
      }
      return this.todoList;
    },

    showCompletedButton(){
      return this.todoList.filter(todo => todo.completed).length > 0;
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
    },

    changeAllTodos() {
      this.todoList.forEach(item => {
        item.completed = event.target.checked;
      });
    },

    clearCompleted() {
      this.todoList = this.todoList.filter(todo => !todo.completed);
    }
  }
};
</script>
