<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo" />
      <h1>Pinia Tasks</h1>
    </header>

    <div class="new-task-form">
      <TaskForm />
    </div>

    <nav class="filter">
      <button @click="filterTask('all')">All tasks</button>
      <button @click="filterTask('fav')">Fav tasks</button>
    </nav>

    <div v-if="loading">
      <h3 class="loading">Loading...</h3>
    </div>

    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ taskCount }} tasks left to do</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="task-list" v-if="filter === 'fav'">
      <p>You have {{ favCount }} fav tasks left to do</p>
      <div v-for="task in fav" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="reset-btn-container">
      <button class="reset" @click="$reset">reset state</button>
    </div>
  </main>
</template>

<script>
import { useTaskStore } from "./stores/TaskStore";
import TaskDetails from "./components/TaskDetails.vue";
import TaskForm from "./components/TaskForm.vue";
import { onMounted, ref } from "vue";
import { storeToRefs } from "pinia";
export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const filter = ref("all");
    const taskStore = useTaskStore();

    const { tasks, loading, fav, taskCount, favCount } = storeToRefs(taskStore);

    const filterTask = (event) => {
      filter.value = event;
    };

    onMounted(() => {
      taskStore.getTask();
    });

    return {
      taskStore,
      filter,
      filterTask,
      tasks,
      loading,
      fav,
      taskCount,
      favCount,
    };
  },
};
</script>
