<script setup>
import { ref } from 'vue';

const props = defineProps({
  job: {
    type: Object,
    default: null,
  },
});
</script>

<template>
  <div class="job-detail-container">
    <div v-if="job" class="row">
      <div class="col-12">
        <div class="card shadow-sm border-0">
          <div class="card-header bg-white border-0 pt-4 px-4">
            <div class="d-flex justify-content-between align-items-start flex-wrap gap-2">
              <div>
                <h3 class="card-title mb-1 fw-bold">{{ job.job_title }}</h3>
                <p class="text-muted mb-0">{{ job.company }}</p>
              </div>
              <span class="badge rounded-pill px-3 py-2" style="background: var(--primary-gradient); color: var(--primary-yellow);">
                {{ job.category }}
              </span>
            </div>
          </div>

          <div class="card-body px-4">
            <div class="row g-3 mb-4">
              <div class="col-md-6 col-lg-4" v-for="(label, key) in metaFields" :key="key">
                <div class="p-3 bg-light rounded-3 h-100">
                  <div class="text-muted small text-uppercase fw-semibold mb-1">{{ label }}</div>
                  <div class="fw-medium">{{ job[key] }}</div>
                </div>
              </div>
            </div>

            <div class="mb-4">
              <h6 class="fw-bold text-uppercase text-muted small mb-2">Required Skills</h6>
              <div class="d-flex flex-wrap gap-2">
                <span v-for="skill in job.required_skills" :key="skill" class="badge rounded-pill px-3 py-2" style="background: #f8f9fa; color: #1a1a1a; border: 1px solid #dee2e6;">
                  {{ skill }}
                </span>
              </div>
            </div>

            <div class="mb-4">
              <h6 class="fw-bold text-uppercase text-muted small mb-2">Preferred Qualifications</h6>
              <div class="d-flex flex-wrap gap-2">
                <span v-for="qual in job.preferred_qualifications" :key="qual" class="badge rounded-pill px-3 py-2" style="background: #1a1a1a; color: #ffc107; border: 1px solid #1a1a1a;">
                  {{ qual }}
                </span>
              </div>
            </div>

            <div class="mb-4">
              <h6 class="fw-bold text-uppercase text-muted small mb-2">Job Description</h6>
              <p class="text-secondary lh-lg mb-0">{{ job.job_description }}</p>
            </div>

            <div>
              <h6 class="fw-bold text-uppercase text-muted small mb-2">Tags</h6>
              <div class="d-flex flex-wrap gap-2">
                <span v-for="tag in job.tags" :key="tag" class="badge rounded-pill px-3 py-2" style="background: #ffc107; color: #1a1a1a; border: 1px solid #ffc107;">
                  #{{ tag }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="row">
      <div class="col-12">
        <div class="alert border-0 shadow-sm" style="background: var(--primary-gradient); color: var(--primary-yellow);">
          <h4 class="alert-heading">Select a job</h4>
          <p class="mb-0" style="color: rgba(255,255,255,0.8);">Choose a position from the sidebar to view its details.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    const metaFields = {
      job_id: 'Job ID',
      employment_type: 'Employment Type',
      salary_range: 'Salary Range',
      job_level: 'Job Level',
      posted_date: 'Posted Date',
      application_deadline: 'Application Deadline',
      start_date: 'Start Date',
      supervisor: 'Supervisor',
      positions_available: 'Positions Available',
      location: 'Location'
    };
    return { metaFields };
  }
};
</script>

<style scoped>
.job-detail-container {
  animation: fadeIn 0.4s ease;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
