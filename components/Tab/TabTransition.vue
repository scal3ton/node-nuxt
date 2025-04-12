<!-- Transitions -->
<!-- Apply transitions between pages and layouts with Vue or native browser View Transitions. -->
<!-- Nuxt leverages Vue's <Transition> component to apply transitions between pages and layouts. -->
<template>
  <div class="tabs-container">
    <div class="tabs-header">
      <button
        v-for="(tab, index) in tabs"
        :key="index"
        @click="navigateToTab(tab.route)"
        :class="{ 'active-tab': currentRoute === tab.route }"
        class="tab-button"
      >
        {{ tab.title }}
      </button>
    </div>

    <div class="tab-content">
      <component :is="tabs.find(tab => tab.route === currentRoute)?.component" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, markRaw, onMounted } from 'vue';
import { useRouter, useRoute } from 'nuxt/app';
import type { Component } from 'vue';

// Import the tab components
import Tab1Content from './Tab1Content.vue';
import Tab2Content from './Tab2Content.vue';
import Tab3Content from './Tab3Content.vue';

// Define the Tab type with Component and route
interface Tab {
  title: string;
  component: Component;
  route: string;
}

// Create the tab data with routes
const tabs = ref<Tab[]>([
  {
    title: 'Tab 1',
    component: markRaw(Tab1Content),
    route: '/tab1',
  },
  {
    title: 'Tab 2',
    component: markRaw(Tab2Content),
    route: '/tab2',
  },
  {
    title: 'Tab 3',
    component: markRaw(Tab3Content),
    route: '/tab3',
  },
]);

// Track the current route
const router = useRouter();
const route = useRoute();
const currentRoute = ref(route.path);

// Function to navigate to a tab's route
const navigateToTab = (routePath: string) => {
  router.push(routePath);
  currentRoute.value = routePath;
};

// Update currentRoute on route changes
onMounted(() => {
  currentRoute.value = route.path;
});
</script>

<style scoped>
.tabs-container {
  max-width: 800px;
  margin: 0 auto;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', sans-serif;
}

.tabs-header {
  display: flex;
  border-bottom: 1px solid #e2e8f0;
  margin-bottom: 1rem;
}

.tab-button {
  padding: 0.75rem 1.5rem;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  color: #4a5568;
  transition: all 0.2s ease;
  border-bottom: 2px solid transparent;
}

.tab-button:hover {
  color: #2d3748;
}

.active-tab {
  color: #3182ce;
  border-bottom: 2px solid #3182ce;
  font-weight: 500;
}

.tab-content {
  padding: 1rem;
  background-color: #f8fafc;
  border-radius: 0.5rem;
  min-height: 200px;
}
</style>