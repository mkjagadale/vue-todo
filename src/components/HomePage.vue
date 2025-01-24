<template>
  <div class="container d-flex justify-content-center align-items-center">
    <div class="card mt-3 col-6">
      <div class="card-body">
        <h1 class="text-center">To-Do List</h1>
        <div
          class="d-flex flex-column flex-sm-row justify-content-between mt-3 mt-sm-5"
        >
          <div class="col-sm-9">
            <input
              type="text"
              class="form-control"
              placeholder="Add a new task..."
              v-model="newTask"
              @keyup.enter="addTask()"
            />
          </div>
          <div class="mt-3 mt-sm-0">
            <button type="button" class="btn btn-primary" @click="addTask()">
              Add
            </button>
          </div>
        </div>
        <div
          class="mt-3 mt-sm-5"
          style="margin: 20px 0 0 20px; width: 90%; text-align: left"
        >
          <div>
            <ul class="list-group my-2">
              <li
                v-for="(item, index) in tasks"
                :key="index"
                class="list-group-item d-flex justify-content-between align-items-center"
              >
                <span
                  v-if="item.isDone"
                  style="display: inline-block; width: 83%"
                >
                  <s> {{ item.description }}</s>
                </span>
                <span
                  v-if="!item.isDone"
                  style="display: inline-block; width: 83%"
                >
                  {{ item.description }}
                </span>
                <span>
                  <button
                    type="button"
                    class="btn btn-secondary mr-2"
                    style="margin-right: 1%"
                  >
                    Edit
                  </button>
                  <button
                    type="button"
                    class="btn btn-danger"
                    @click="deleteTask(index)"
                  >
                    Delete
                  </button>
                </span>
              </li>
            </ul>
          </div>
        </div>
        <!-- Rest of the content -->
      </div>
    </div>
  </div>
</template>
<script>
import { ref, onMounted, computed, watch } from "vue";

export default {
  name: "HomePage",
  setup() {
    const newTask = ref("");
    const tasks = ref([
      { description: "Review pending activities", isDone: false },
      { description: "Attend daily meetings", isDone: false },
    ]);

    const addTask = () => {
      if (!newTask.value) return;
      tasks.value.unshift({
        description: newTask.value,
        isDone: false,
      });
      localStorage.setItem("tasks", JSON.stringify(tasks.value));
      newTask.value = "";
    };

    const deleteTask = (index) => {
      console.log("deleteing Task", index);
      tasks.value.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(tasks.value));
    };

    const deleteAllTasks = () => {
      tasks.value = [];
      localStorage.removeItem("tasks");
    };

    const pendingTasks = computed(() => {
      return tasks.value.filter((x) => x.isDone === false).length;
    });

    watch(tasks, { deep: true });

    onMounted(() => {
      if (localStorage.tasks) {
        tasks.value = JSON.parse(localStorage.getItem("tasks")) || [];
      }
    });

    return {
      newTask,
      tasks,
      addTask,
      deleteTask,
      deleteAllTasks,
      pendingTasks,
    };
  },
};
</script>
<style scoped>
.card-body h1 {
  color: #a813ff;
}

.btn-primary {
  background-color: #a813ff;
  border: transparent;
}

.btn-danger {
  background-color: #5f8dba;
  border: transparent;
}

.btn-warning {
  background-color: #952b95;
  color: #fff;
  border: transparent;
}

.message {
  color: green;
  font-weight: bold;
}

.item-card {
  height: 60px;
  background-color: #440c7e;
  color: #fff;
}
</style>
