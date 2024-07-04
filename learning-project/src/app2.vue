
<script>
  export default {
    data() {
      return {
        name: 'John Doe',
        status: 'pending',
        tasks: ['Task One', 'Task Two', 'Task Three'],
        link: 'https://www.google.com.br/'
      }
    },
    methods: {
      toggleStatus() {
        if (this.status === 'active') {
          this.status = 'pending';

        } else if (this.status === 'pending') {
          this.status = 'inactive'
        } else {
          this.status = 'active';
        }

      }
    }
  }
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'" >User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>
  <a :href="link">Click for Google</a>
  <br>
  <br>
  <button @click="toggleStatus">Change Status</button>
</template>


<script setup>
import { onMounted, ref } from 'vue';

    const name = ref('John Doe');
    const status = ref('active');
    const tasks = ref(['Task One', 'Task Two', 'Task Three']);
    const newTask = ref('');
    
    const toggleStatus = () => {
      if (status.value === 'active') {
          status.value = 'pending';

        } else if (status.value === 'pending') {
          status.value = 'inactive'
        } else {
          status.value = 'active';
        }
      }
    const addTask = () => {
      if (newTask.value.trim() !== '') {
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
    };
    const deleteTask = (index) => {
      tasks.value.splice(index, 1);
    }

    onMounted(async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos');
        const data = await response.json();
        tasks.value = data.map((task) => task.title);
      } catch (error) {
        console.log('Error fetching tasks');
      }
    });
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'" >User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" name="newTask" id="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>
  <br>
  <br>
  <button @click="toggleStatus">Change Status</button>
</template>



