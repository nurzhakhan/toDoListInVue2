<template>
  <div id="todolist">

    <h1 class="title">{{ title }}</h1>

    <p v-if="isEmpty">To Do List is empty</p>

    <div class="toDo-container">
      <div class="container" v-for="(task, index) in tasks">
        <div class="display-col">
            <span v-if="selectedIndex!==index" class="task-name">
            {{ task.title }}
            </span>
          <input v-if="selectedIndex===index" v-model="selectedTaskTitle"/>

          <span v-if="selectedIndex!==index" :class="task.category">
            {{ task.category }}
        </span>
          <select v-if="selectedIndex===index" v-model="selectedCategory">
            <option disabled selected hidden>{{ task.category }}</option>
            <option v-for="category in categories" :value="category" :key="category">{{ category }}</option>
          </select>
        </div>
        <div class="btn-display-row">
          <!--          edit btn-->
          <button class="btn btn-link btn-color bi mr-10 " :class="(selectedIndex === index) ? 'bi-check-lg' : 'bi-pencil' "
                  @click="(selectedIndex === index) ? saveTask() : editTask(index)">
          </button>
          <!--          delete btn-->
          <button class="btn btn-link btn-color bi bi-trash3 mr-10 " @click="deleteTask(index)">
          </button>
        </div>
      </div>

      <div>
        <button
            id="open-modal"
            class="btn btn-link btn-add bi"
            :class="{'bi-plus-circle-fill': filledAddIcon, 'bi-plus-circle': !filledAddIcon}"
            @mouseover="onMouseOver" @mouseout="onMouseOut"
            @click="open()">
        </button>
        <AddTaskForm v-if="isModalOpened" @closeEvent="close" @newTask="getNewTask"/>
      </div>
    </div>


  </div>
</template>

<script>
import AddTaskForm from "@/components/AddTaskForm.vue";
import axios from "axios";

export default {
  components: {AddTaskForm},
  name: "ToDoList",
  data() {
    return {
      title: "To Do List",
      //tasks: [{id: 1, title: "Buy groceries", category: "high"}, {id: 2, title: "Buy xxxx", category: "low"}],
      tasks: [],
      categories: ["high", "medium", "low"],
      filledAddIcon: true,
      isModalOpened: false,
      isEdit: false,
      selectedTaskTitle: "",
      selectedCategory: "",
      selectedIndex: -1,
    }
  },
  created() {
    this.getToDoList();
  },
  computed: {
    isEmpty() {
      return this.tasks.length === 0;
    }
  },
  methods: {
    onMouseOver() {
      this.filledAddIcon = true;
    },
    onMouseOut() {
      this.filledAddIcon = false;
    },
    open() {
      this.isModalOpened = true;
    },
    close() {
      this.isModalOpened = false;
    },
    getNewTask(data) {
      console.log("Data from child " + data);
      this.tasks.push(data);
    },
    getToDoList() {
      axios.get('https://jsonplaceholder.typicode.com/todos')
          .then(response => {
            for (let i = 0; i < 4; i++) {
            console.log(response.data[i]);
              this.tasks.push({
                id: response.data[i].id,
                title: response.data[i].title,
                category: 'high'
              });
            }
          })
          .catch(error => {
            console.error('Error: ', error);
          });
    },
    editTask(index) {
      this.selectedIndex = index;
      this.selectedTaskTitle = this.tasks[index].title;
      this.selectedCategory = this.tasks[index].category;
      console.log('Edit todo ' + index);
    },
    saveTask() {
      this.tasks[this.selectedIndex].title = this.selectedTaskTitle;
      this.tasks[this.selectedIndex].category = this.selectedCategory;
      this.selectedIndex = -1;
    },
    deleteTask(index) {
      const todoId = this.tasks[index].id;
      this.tasks.splice(index, 1);
      console.log(todoId);
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${todoId}`)
          .then(response => {
            console.log('Todo deleted:', response.data);
          })
          .catch(error => {
            console.error('Error deleting todo:', error);
          });
    }
  }
}
</script>

<style scoped>
#todolist {
  display: inline-block;
}

@media (max-width: 720px) {
  .container {
    flex-direction: column;
  }
}

.title {
  font-family: 'Inter';
  margin: 80px 0 40px 0;
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  line-height: 34px;
  color: black;
}

.container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  width: 70vw;
  background: #FFFFFF;
  box-shadow: 0 0 9px #E6E6E6;
  border-radius: 12px;
  margin-bottom: 20px;
}

.toDo-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 80px;
}

.display-col {
  display: flex;
  flex-direction: column;
}

.btn-display-row {
  display: flex;
  flex-direction: row;
}

.mr-10 {
  margin: 10px 5px;
}

select {
  border-radius: 6px;
  border: 1px solid #E5F2FF;
}

input {
  font-size: 15px;
  margin-bottom: 3px;
  height: 16px;
  background: #FFFFFF;
  border: 1px solid #E5F2FF;
  border-radius: 6px;
  padding: 14px 10px;
}

option {
  font-style: normal;
  font-weight: 600;
  font-size: 15px;
  color: #7A93B8;
}

.task-name {
  font-style: normal;
  font-weight: 400;
  font-size: 18px;
  line-height: 22px;
  color: #0F0E17;
  margin-bottom: 9px;
}

.high {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding: 4px 8px;
  width: 43px;
  height: 24px;
  background: #FCEEF5;
  border-radius: 4px;
  font-style: normal;
  font-weight: 600;
  font-size: 12px;
  line-height: 16px;
  color: #571032;
}

.medium {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding: 4px 8px;
  width: 43px;
  height: 24px;
  background: #EEF3FC;
  border-radius: 4px;
  font-style: normal;
  font-weight: 600;
  font-size: 12px;
  line-height: 16px;
  color: #103D57;
}

.low {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding: 4px 8px;
  width: 43px;
  height: 24px;
  background: #EFFCEE;
  border-radius: 4px;
  font-style: normal;
  font-weight: 600;
  font-size: 12px;
  line-height: 16px;
  color: #14A35E;
}

.btn-add {
  color: #E53170;
  font-size: 70px;
}

.btn-color {
  color: #F25F4C;
}

.btn-color:hover {
  color: white;
  background: #F25F4C;
}
</style>