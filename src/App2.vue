<!-- <script>
  import { ref } from 'vue';

  export default {
    setup() {
      const name = ref('John');
      const status = ref('pending');
      const tasks = ref(['Task One', 'Task Two', 'Task Three']);
      const link = ref('https://www.youtube.com/watch?v=VeNfHj6MhgA&t=1480s');

      const toggleStatus = () => {
        if (status.value === 'active'){
          status.value = 'pending';
        } else if (status.value === 'pending'){
          status.value = 'inactive';
        } else {
          status.value = 'active';
        }
      };

      return {
        name,
        status,
        tasks,
        link,
        toggleStatus,
      };
    }
  };
</script> -->

<script setup>
  import { ref, onMounted } from 'vue';

      const name = ref('John');
      const status = ref('pending');
      const tasks = ref(['Task One', 'Task Two', 'Task Three']);
      const link = ref('https://www.youtube.com/watch?v=VeNfHj6MhgA&t=1480s');
      const newTask = ref('');

      const toggleStatus = () => {
        if (status.value === 'active'){
          status.value = 'pending';
        } else if (status.value === 'pending'){
          status.value = 'inactive';
        } else {
          status.value = 'active';
        }
      };

const addTask = () => {
  if(newTask.value.trim() !== ''){
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
};

const deleteTask = (index) => {
    tasks.value.splice(index, 1)
}

onMounted(async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((task) => task.title);
    } catch (error) {
      console.log(error);
    }
})
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is Inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

  <h2>Tasks</h2>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <a :href="link">Click for link</a>
  <br>
  <button @click="toggleStatus">Change Status</button>
</template>

