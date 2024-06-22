<template>
  <form @submit.prevent="handleSubmit">
    <input type="text" placeholder="I need to..." v-model="newTask" />
    <button>Add</button>
  </form>
</template>

<script>
import { ref } from "vue";
import { useTaskStore } from "@/stores/TaskStore";
export default {
  setup() {
    const newTask = ref("");
    const taskStore = useTaskStore();

    const handleSubmit = () => {
      if (newTask.value.length > 0) {
        newTask.value =
          newTask.value.charAt(0).toUpperCase() +
          newTask.value.slice(1).toLowerCase();

        taskStore.addTask({
          title: newTask.value,
          isFav: false,
          id: Math.floor(Math.random() * 10000),
        });
      }
      newTask.value = "";
    };

    return { newTask, handleSubmit };
  },
};
</script>
