<template>
  <div class="app">
    <header>
      <div class="order">
        <button @click="handleClick('title')">Order by title</button>
        <button @click="handleClick('salary')">Order by salary</button>
        <button @click="handleClick('location')">Order by location</button>
      </div>
    </header>
    <div class="inputText">
      <input
        type="text"
        placeholder="Enter your Title"
        name="title"
        v-model="inputJobs.title"
      />
      <input
        type="number"
        placeholder="Enter your Salary"
        name="salary"
        v-model="inputJobs.salary"
      />
      <input
        type="text"
        placeholder="Enter your Location "
        name="location"
        v-model="inputJobs.location"
      />
      <div>
        <button @click="handleAdd">Add</button>
      </div>
    </div>
    <JobsList :jobs="jobs" :order="order" />
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import JobsList from "./components/JobsList.vue";
import Job from "./types/Job";
import OrderTerm from "./types/OrderTerm";
import axios from "axios";

const jobs = ref<Job[]>([]);
const inputJobs = ref<Job>({
  id: Math.floor(Math.random() * 1000),
  title: "",
  salary: 0,
  location: "",
});

watchEffect(async () => {
  const response = await axios.get(`http://localhost:3000/jobs`);
  jobs.value = await response.data;
});

const order = ref<OrderTerm>("title");

const handleClick = (term: OrderTerm) => {
  order.value = term;
};

const handleAdd = async () => {
  try {
    await axios.post(`http://localhost:3000/jobs`, inputJobs.value);

    jobs.value.push(inputJobs.value);

    inputJobs.value = {
      ...inputJobs.value,
      title: "",
      salary: 0,
      location: "",
    };
  } catch (error) {
    console.log(error);
  }
};
</script>

<style scoped>
header {
  text-align: center;
}
header .order {
  margin-top: 20px;
}
button {
  margin: 0 10px;
  color: #1195c9;
  border: 3px solid #1195c9;
  background: #d5f0ff;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}
.inputText {
  margin: 20px 0;
  display: flex;
  justify-content: center;
}
input {
  margin: 10px 10px;
}
</style>
