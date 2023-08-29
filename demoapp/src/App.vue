<template>
  <div class="app">
    <h1 class="app-title">TODO APP</h1>
    <div class="input-container">
      <input v-model="newTask" class="task-input" placeholder="Add a new task" @keyup.enter="addTask" />
      <input v-model="newTaskTime" class="time-input" type="time" />
      <button class="add-button" @click="addTask">Add</button>
    </div>
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" class="task-item">
        <span class="task-text">{{ task.text }}</span>
        <span v-if="task.isCompleted" class="task-completed">Task Completed</span>
        <button class="edit-button" @click="editTask(index)">Edit</button>
        <button class="delete-button" @click="removeTask(index)">Delete</button>
        <span v-if="task.isGoal" class="task-time">{{ task.time }}</span>
        <input v-if="task.isGoal" v-model="task.completed" type="checkbox" @change="completeTask(index)" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      newTaskTime: '',
      tasks: [],
      editIndex: -1,
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        const task = { text: this.newTask, time: this.newTaskTime, isGoal: true, completed: false };
        if (this.editIndex === -1) {
          this.tasks.push(task);
        } else {
          this.tasks.splice(this.editIndex, 1, task);
          this.editIndex = -1;
        }
        this.newTask = '';
        this.newTaskTime = '';
      }
    },
    editTask(index) {
      this.editIndex = index;
      const task = this.tasks[index];
      this.newTask = task.text;
      this.newTaskTime = task.time;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      if (this.editIndex === index) {
        this.editIndex = -1;
        this.newTask = '';
        this.newTaskTime = '';
      }
    },
    completeTask(index) {
      this.tasks[index].isCompleted = true;
    },
  },
};
</script>

<style>
.app {
  background-color: rgba(90, 215, 240, 0.685);
  width: 500px;
  height: 400px;
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #da0a0a;
  border-radius: 5px;
  font-family: Arial, sans-serif;
}

.app-title {
  text-align: center;
  margin-bottom: 20px;
}

.input-container {
  display: flex;
  margin-bottom: 10px;
}

.task-input,
.time-input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 3px;
  margin-right: 5px;
}

.add-button {
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 3px;
  padding: 8px 15px;
  cursor: pointer;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  border: 1px solid #2f2020;
  border-radius: 3px;
  margin: 5px 0;
}

.delete-button,
.edit-button {
  background-color: #f44336;
  color: white;
  border: none;
  border-radius: 3px;
  padding: 5px 10px;
  cursor: pointer;
  margin-left: 5px;
}

.edit-button {
  background-color: #ffc107;
}

.task-completed {
  color: green;
  font-weight: bold;
  margin-left: 10px;
}

.task-time {
  margin-left: 10px;
}
</style>
