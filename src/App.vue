<template>
  <main>
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo" />
      <h1>Pinia Tasks</h1>
    </header>
    <nav class="filter">
      <button @click="filter = 'all'">All Tasks</button>
      <button @click="filter = 'fav'">Fav Tasks</button>
    </nav>

    <TaskForm />

    <div class="loading" v-if="loading">Loading...</div>
    <template v-else>
      <div class="task-list" v-if="filter === 'all'">
        <p>You have {{ totalCount }} tasks to do.</p>
        <div v-for="task in tasks" :key="task.id">
          <TaskDetails :task="task" />
        </div>
      </div>
      <div class="task-list" v-if="filter === 'fav'">
        <p>You have {{ favCount }} fav tasks to do.</p>

        <div v-for="task in favs" :key="task.id">
          <TaskDetails :task="task" />
        </div>
      </div>
    </template>
    <button @click="taskStore.$reset">Reset</button>
  </main>
</template>

<script>
import { ref } from "vue";
import { useTaskStore } from "./stores/TaskStore";
import TaskDetails from "./components/TaskDetails.vue";
import TaskForm from "./components/TaskForm.vue";
import { storeToRefs } from "pinia";

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore();

    const { loading, totalCount, tasks, favCount, favs } =
      storeToRefs(taskStore);

    taskStore.getTasks();

    const filter = ref("all");

    return { filter, loading, totalCount, tasks, favCount, favs };
  },
};
</script>
