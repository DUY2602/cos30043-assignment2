<script setup>
import { ref } from 'vue';

const tasks = ref([]);
const newTask = ref('');

function addTask() {
  const text = newTask.value.trim();
  if (text) {
    tasks.value.unshift({
      id: Date.now(),
      text: text,
      priority: 'Low'
    });
    newTask.value = '';
  }
}

function deleteTask(index) {
  tasks.value.splice(index, 1);
}

function togglePriority(index) {
  const task = tasks.value[index];
  task.priority = task.priority === 'Low' ? 'High' : 'Low';
}

function getPriorityButtonText(priority) {
  return priority === 'Low' ? 'Mark as High Priority' : 'Mark as Low Priority';
}

function getPriorityBadgeClass(priority) {
  return priority === 'High' ? 'bg-danger' : 'bg-success';
}
</script>

<template>
  <div class="todo-container">
    <div class="text-center mb-4">
      <h2 class="fw-bold">To-Do List</h2>
      <p class="text-muted">Stay organized. Add, prioritize, and manage your tasks.</p>
    </div>

    <div class="row justify-content-center">
      <div class="col-lg-10">
        <div class="card border-0 shadow-sm">
          <div class="card-body p-4 p-md-5">
            <div class="input-group mb-4 input-group-lg">
              <input
                type="text"
                v-model="newTask"
                @keyup.enter="addTask"
                class="form-control"
                placeholder="What needs to be done?"
                aria-label="New task"
              >
              <button class="btn btn-primary px-4" type="button" @click="addTask">
                Add
              </button>
            </div>

            <div v-if="tasks.length === 0" class="text-center py-5 text-muted">
              <p class="mb-0">No tasks yet. Add one above to get started!</p>
            </div>

            <div v-else class="list-group list-group-flush">
              <div
                v-for="(task, index) in tasks"
                :key="task.id"
                class="list-group-item d-flex justify-content-between align-items-center py-3 px-0 border-bottom"
                :class="{ 'bg-light': task.priority === 'High' }"
              >
                <div class="d-flex align-items-center gap-3 flex-grow-1">
                  <span class="badge rounded-pill fs-6 px-3 py-2" :class="getPriorityBadgeClass(task.priority)">
                    {{ task.priority === 'High' ? 'High' : 'Low' }}
                  </span>
                  <span class="fw-medium fs-5">
                    {{ task.text }}
                    <small class="text-muted ms-1">
                      {{ task.priority === 'High' ? '(High Priority)' : '(Low Priority)' }}
                    </small>
                  </span>
                </div>
                <div class="btn-group btn-group-sm ms-3">
                  <button
                    class="btn btn-outline-secondary"
                    @click="togglePriority(index)"
                    :title="getPriorityButtonText(task.priority)"
                  >
                    {{ getPriorityButtonText(task.priority) }}
                  </button>
                  <button
                    class="btn btn-outline-danger"
                    @click="deleteTask(index)"
                    title="Delete task"
                  >
                    Delete
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.todo-container {
  animation: fadeIn 0.4s ease;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
.list-group-item {
  transition: background-color 0.15s ease;
}
.list-group-item:hover {
  background-color: #f8f9fa !important;
}
.btn-group-sm .btn {
  padding: 0.35rem 0.75rem;
  font-size: 0.875rem;
}
</style>
