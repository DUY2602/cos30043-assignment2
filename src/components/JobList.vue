<script setup>
import { ref } from 'vue';

const props = defineProps({
  jobs: {
    type: Array,
    required: true,
  },
  selectedJobId: {
    type: String,
    default: null,
  },
});

const emit = defineEmits(['select-job', 'show-overview']);
</script>

<template>
  <div class="job-list-sidebar">
    <div class="list-group list-group-flush">
      <button
        type="button"
        class="list-group-item list-group-item-action overview-btn d-flex align-items-center"
        :class="{ active: selectedJobId === null }"
        @click="emit('show-overview')"
      >
        <div>
          <div class="fw-semibold">Overview</div>
          <small class="text-muted">Welcome and stats</small>
        </div>
      </button>
      <button
        v-for="job in jobs"
        :key="job.job_id"
        type="button"
        class="list-group-item list-group-item-action job-btn d-flex align-items-center"
        :class="{ active: selectedJobId === job.job_id }"
        @click="emit('select-job', job.job_id)"
      >
        <div class="flex-grow-1 overflow-hidden">
          <div class="fw-semibold text-truncate">{{ job.job_title }}</div>
          <small :class="selectedJobId === job.job_id ? 'text-white-50' : 'text-muted'">
            {{ job.job_id }} · {{ job.location }}
          </small>
        </div>
        <span
          class="badge ms-2"
          :class="selectedJobId === job.job_id ? 'bg-light text-dark' : 'bg-dark text-warning'"
        >
          {{ job.category }}
        </span>
      </button>
    </div>
  </div>
</template>

<style scoped>
.job-list-sidebar {
  overflow-y: auto;
  height: 100%;
}
.list-group-item {
  padding: 0.75rem 1rem;
  border-bottom: 1px solid #f1f3f5;
  transition: all 0.15s ease;
}
.overview-btn {
  font-weight: 600;
  background-color: #f8f9fa;
  border-bottom: 2px solid #e9ecef;
}
.overview-btn:hover {
  background-color: #e9ecef;
}
.overview-btn.list-group-item.active,
.list-group-item.overview-btn.active {
  background: var(--primary-gradient);
  color: #ffffff !important;
  border-bottom: 2px solid #ffc107;
}
.list-group-item.overview-btn.active small {
  color: rgba(255, 255, 255, 0.75) !important;
}
.job-btn:hover {
  background-color: #f8f9fa;
  padding-left: 1.25rem;
}
.job-btn.active {
  background: var(--primary-gradient);
  color: var(--primary-yellow);
  border-left: 4px solid #ffc107;
}
.job-btn.active .text-muted,
.job-btn.active small {
  color: rgba(255, 255, 255, 0.7) !important;
}
</style>
