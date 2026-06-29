<script setup>
import { ref, nextTick, onMounted, onUnmounted } from 'vue';
import JobExplorer from './components/JobExplorer.vue';
import ApplicationForm from './components/ApplicationForm.vue';
import ToDoList from './components/ToDoList.vue';

const activeSection = ref('job-explorer');
const isNavOpen = ref(false);

const sections = [
  { id: 'job-explorer', label: 'Job Explorer' },
  { id: 'apply', label: 'Apply for Job' },
  { id: 'todo', label: 'To-Do List' }
];

function scrollTo(id) {
  activeSection.value = id;
  isNavOpen.value = false;
  nextTick(() => {
    const el = document.getElementById(id);
    if (el) {
      const navHeight = 80;
      const top = el.getBoundingClientRect().top + window.scrollY - navHeight;
      window.scrollTo({ top, behavior: 'smooth' });
    }
  });
}

function handleScroll() {
  const scrollPos = window.scrollY + 120;
  for (let i = sections.length - 1; i >= 0; i--) {
    const el = document.getElementById(sections[i].id);
    if (el && el.offsetTop <= scrollPos) {
      activeSection.value = sections[i].id;
      break;
    }
  }
}

let scrollHandler = null;

onMounted(() => {
  scrollHandler = handleScroll;
  window.addEventListener('scroll', scrollHandler, { passive: true });
});

onUnmounted(() => {
  if (scrollHandler) {
    window.removeEventListener('scroll', scrollHandler);
  }
});
</script>

<template>
  <div class="app-root">
    <nav class="navbar navbar-expand-lg navbar-dark shadow-sm sticky-top" style="background: linear-gradient(90deg, #1a1a1a, #2d2d2d);">
      <div class="container">
        <a class="navbar-brand fw-bold" href="#">
          Insight Hire
        </a>
        <button
          class="navbar-toggler"
          type="button"
          @click="isNavOpen = !isNavOpen"
          :aria-expanded="isNavOpen"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" :class="{ show: isNavOpen }" id="mainNavbar">
          <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
            <li class="nav-item" v-for="section in sections" :key="section.id">
              <a
                class="nav-link"
                :class="{ active: activeSection === section.id }"
                href="#"
                @click.prevent="scrollTo(section.id)"
              >
                {{ section.label }}
              </a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <section id="job-explorer" class="section-block">
      <div class="container">
        <JobExplorer />
      </div>
    </section>

    <section id="apply" class="section-block bg-light">
      <div class="container">
        <ApplicationForm />
      </div>
    </section>

    <section id="todo" class="section-block">
      <div class="container">
        <ToDoList />
      </div>
    </section>

    <footer class="bg-dark text-light py-4 mt-auto">
      <div class="container text-center">
        <p class="mb-0">&copy; 2025 Insight Hire. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.app-root {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.section-block {
  flex: 1;
  padding: 3rem 0;
}
.navbar-brand {
  font-size: 1.4rem;
  letter-spacing: 0.5px;
}
.nav-link {
  font-weight: 500;
  margin: 0 0.25rem;
  border-radius: 0.375rem;
  transition: all 0.2s ease;
}
.nav-link:hover {
  background-color: rgba(255, 193, 7, 0.15);
}
.nav-link.active {
  background-color: rgba(255, 193, 7, 0.25);
  color: #ffc107 !important;
  font-weight: 600;
}
</style>
