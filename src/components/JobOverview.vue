<script setup>
const overviewMessage = 'Welcome to Insight Hire! Use the sidebar to explore available job positions and view detailed information.';
</script>

<template>
  <div class="overview-container">
    <div class="card border-0 shadow-sm text-center py-5 px-4">
      <div class="card-body py-5">
        <h2 class="fw-bold mb-3">Job Overview</h2>
        <p class="lead text-muted mb-4">{{ overviewMessage }}</p>
        <div class="d-flex justify-content-center gap-3 flex-wrap">
          <div class="p-3 bg-light rounded-3 border" style="min-width: 140px;">
            <div class="fs-3 fw-bold" style="color: #1a1a1a;">10</div>
            <div class="text-muted small text-uppercase fw-semibold">Open Positions</div>
          </div>
          <div class="p-3 bg-light rounded-3 border" style="min-width: 140px;">
            <div class="fs-3 fw-bold" style="color: #1a1a1a;">5</div>
            <div class="text-muted small text-uppercase fw-semibold">Categories</div>
          </div>
          <div class="p-3 bg-light rounded-3 border" style="min-width: 140px;">
            <div class="fs-3 fw-bold" style="color: #ffc107;">100%</div>
            <div class="text-muted small text-uppercase fw-semibold">Remote Friendly</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.overview-container {
  animation: fadeIn 0.4s ease;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
