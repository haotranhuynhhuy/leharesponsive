<template>
  <div class="jobslist">
    <p>Order by {{ order }}</p>
    <ul>
      <li v-for="job in orderedJob" :key="job.id">
        <h2>{{ job.title }} in {{ job.location }}</h2>
        <div class="salary">
          <p>${{ job.salary }}</p>
        </div>
        <div class="description">
          <p>
            Lorem ipsum dolor sit, amet consectetur adipisicing elit. Et
            repellat possimus, inventore voluptate nihil dicta rem. Distinctio
            eos culpa doloremque assumenda consequatur at nihil quibusdam cumque
            maxime, quia, ducimus exercitationem.
          </p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import Job from "@/types/Job";
import OrderTerm from "@/types/OrderTerm";
import { PropType, defineProps, computed } from "vue";

// export default defineComponent({
//   props: {
//     jobs: {
//       type: Array as PropType<Job[]>,
//       required: true,
//     },
//     order: {
//       type: String as PropType<OrderTerm>,
//       required: true,
//     },
//   },
// });
const props = defineProps({
  jobs: {
    required: true,
    type: Array as PropType<Job[]>,
  },
  order: {
    required: true,
    type: String as PropType<OrderTerm>,
  },
});
const orderedJob = computed(() => {
  return [...props.jobs].sort((a: Job, b: Job) => {
    return a[props.order] > b[props.order] ? 1 : -1;
  });
});
</script>

<style scoped>
.jobslist {
  max-width: 960px;
  margin: 40px auto;
}
.jobslist ul {
  padding: 0;
}
.jobslist li {
  background-color: white;
  list-style: none;
  margin-bottom: 20px;
  padding: 16px;
  border-radius: 4px;
}
.jobslist h2 {
  margin: 0 0 10px;
  text-transform: capitalize;
}
.salary {
  display: flex;
}
.salary p {
  color: #17bf66;
  font-weight: bold;
  margin: 10px 4px;
}
</style>
