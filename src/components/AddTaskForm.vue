<template>
  <div id="modal" class="bg-modal" >
    <div class="modal-content">
      <div class="modal-header">
        <span class="title">{{ title }}</span>
        <button class="btn btn-link bi bi-x-lg" style="color: black;" @click="close"></button>
      </div>
      <div class="form-content">
        <input type="text" v-model="taskTitle" placeholder="Title">
        <select v-model="selectedCategory">
          <option value="" disabled selected hidden>Category</option>
          <option v-for="category in categories" :value="category" :key="category">{{ category }}</option>
        </select>
      </div>
      <div>
        <button class="btn-create" @click="createTask">
          Create
        </button>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "AddTaskForm",
  data() {
    return {
      title: "ToDo",
      taskTitle: "",
      selectedCategory: "",
      categories: ["high", "medium", "low"],
      onNewTask: {}

    }
  },
  methods: {
    close() {
      this.$emit('closeEvent');
      console.log('AddTask |Form page');
    },
    createTask() {
      if (this.taskTitle.length === 0 || this.selectedCategory === '') return;
      this.onNewTask = {
        id: Date.now(),
        title: this.taskTitle,
        category: this.selectedCategory,
      };
      this.$emit('onNewTask', this.onNewTask);
      this.close();
    }
  }

}
</script>

<style scoped>
.bg-modal {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.4);
  transition: opacity 0.4s ease-in-out, transform 0.4s ease-in-out;
}

.modal-content {
  width: 40%;
  padding: 0 20px 36px;
}

.modal-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 16px 20px;
  background: #FFFFFF;
  border-bottom: 1px solid #E5F2FF;
}

.title {
  font-style: normal;
  font-weight: 700;
  font-size: 20px;
  color: #1C2636;
}

.form-content {
  display: flex;
  flex-direction: column;
  margin: 20px 0;
}

input, select {
  font-size: 15px;
  margin-bottom: 3px;
  height: 64px;
  background: #FFFFFF;
  border: 1px solid #E5F2FF;
  border-radius: 6px;
  padding: 22px 16px;
}

input::placeholder, option, select {
  font-style: normal;
  font-weight: 600;
  font-size: 15px;
  color: #7A93B8;
}

.btn-create {
  width: 100%;
  height: 48px;
  background: #E53170;
  border-radius: 8px;
  color: white;
  border-style: none;
  font-size: 15px;
  font-style: normal;
  font-weight: 600;
}

.btn-create:hover {
  background: white;
  color: #E53170;
  border-color: #E53170;
}
</style>