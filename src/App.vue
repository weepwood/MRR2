<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import AppShell, { type PageKey } from './components/AppShell.vue'
import ArchiveView from './views/ArchiveView.vue'
import MonitoringView from './views/MonitoringView.vue'
import OverviewView from './views/OverviewView.vue'
import RecordsView from './views/RecordsView.vue'
import SettingsView from './views/SettingsView.vue'
import StatisticsView from './views/StatisticsView.vue'

const storedTheme = localStorage.getItem('mrr2-theme')
const darkMode = ref(storedTheme === 'dark')
const activePage = ref<PageKey>('overview')

const viewMap = {
  overview: OverviewView,
  archive: ArchiveView,
  records: RecordsView,
  statistics: StatisticsView,
  monitoring: MonitoringView,
  settings: SettingsView,
}

const currentView = computed(() => viewMap[activePage.value])

watch(darkMode, (value) => {
  document.documentElement.dataset.theme = value ? 'dark' : 'light'
  localStorage.setItem('mrr2-theme', value ? 'dark' : 'light')
}, { immediate: true })
</script>

<template>
  <AppShell
    :active-page="activePage"
    :dark-mode="darkMode"
    @navigate="activePage = $event"
    @toggle-theme="darkMode = !darkMode"
  >
    <component :is="currentView" @navigate="activePage = $event" />
  </AppShell>
</template>
