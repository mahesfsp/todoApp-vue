<template>
  <div class="todo-app">
    <h1>To-Do List</h1>
    <!-- Input for new task with Enter key or button to add task -->
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />
    <button @click="addTask">Add Task</button>

    <!-- List of tasks with editing and deletion options -->
    <ul>
      <li
        v-for="(task, index) in tasks"
        :key="index"
        :class="{ completed: task.completed }"
      >
        <input type="checkbox" v-model="task.completed" />
        <!-- Double-click to edit the task, blur event to save changes -->
        <span
          @dblclick="editTask(index)"
          :contenteditable="task.isEditing"
          @blur="stopEditing(task, index)"
        >
          {{ task.name }} - {{ task.date }}
        </span>
        <button class="delete-btn" @click="deleteTask(index)">Delete</button>
      </li>
    </ul>

    <!-- Display open tasks for easy viewing -->
    <h2>Open Tasks</h2>
    <ul>
      <li v-for="task in openTasks" :key="task.id">{{ task.name }}</li>
    </ul>

    <!-- Error message for user feedback -->
    <div v-if="error" class="error">{{ error }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '', // Holds the value of the new task input
      tasks: [], // Array to store tasks
      error: null, // Error message if needed
    };
  },
  computed: {
    // Computed property to filter open tasks
    openTasks() {
      return this.tasks.filter((task) => !task.completed);
    },
  },
  methods: {
    // Method to add a new task
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({
          name: this.newTask,
          completed: false,
          date: new Date().toLocaleDateString(),
          isEditing: false,
        });
        this.newTask = '';
        this.saveTasks();
      } else {
        this.error = 'Please enter a task name!';
      }
    },
    // Method to edit an existing task
    editTask(index) {
      this.tasks[index].isEditing = true;
    },
    // Save the task after editing
    stopEditing(task) {
      task.isEditing = false;
      this.saveTasks();
    },
    // Delete a task from the list
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    // Save tasks to local storage
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    // Load tasks from local storage on initialization
    loadTasks() {
      const savedTasks = JSON.parse(localStorage.getItem('tasks'));
      if (savedTasks) {
        this.tasks = savedTasks;
      }
    },
  },
  // Lifecycle hook to load tasks when component is mounted
  mounted() {
    this.loadTasks();
  },
};
</script>

<style scoped>
/* Main styling for the To-Do app */
.todo-app {
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
  padding: 30px;
  background: linear-gradient(145deg, #f5f7fa, #c3cfe2);
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  color: #333;
  font-size: 2.5em;
  margin-bottom: 20px;
}

input {
  padding: 15px;
  width: calc(100% - 30px);
  margin-bottom: 20px;
  border: 2px solid #ddd;
  border-radius: 10px;
  font-size: 1em;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #007bff;
  outline: none;
}

button {
  padding: 10px 20px;
  color: #fff;
  background: linear-gradient(145deg, #007bff, #0056b3);
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 1em;
  transition: transform 0.2s, background 0.3s;
}

button:hover {
  background: linear-gradient(145deg, #0056b3, #003f7f);
  transform: translateY(-3px);
}

ul {
  list-style: none;
  padding: 0;
  margin: 20px 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 8px;
  background: #ffffff80;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);
  transition: background 0.3s;
}

li:hover {
  background: #f0f8ff;
}

.completed {
  text-decoration: line-through;
  color: #888;
  opacity: 0.7;
}

.delete-btn {
  background: #ff4d4d;
  transition: transform 0.2s, background 0.3s;
}

.delete-btn:hover {
  background: #e60000;
  transform: translateY(-2px);
}

.error {
  color: #d9534f;
  margin-top: 20px;
  font-weight: bold;
}
</style>
