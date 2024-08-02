<script setup>
import { ref, computed } from 'vue'
import LayoutPage from './layout/LayoutPage.vue'
import PersonalInformationPage from './pages/PersonalInformationPage.vue'
import AccessLevel from './pages/AccessLevel.vue'
import ActivityLog from './pages/ActivityLog.vue'
import ApprovalPage from './pages/ApprovalPage.vue'
import AttendancePage from './pages/AttendancePage.vue'
import CompanyPage from './pages/CompanyPage.vue'
import DashboardPage from './pages/DashboardPage.vue'
import LeavePage from './pages/LeavePage.vue'
import OrganizationPage from './pages/OrganizationPage.vue'
import PayRollPage from './pages/PayRollPage.vue'
import PortalPage from './pages/PortalPage.vue'
import NotFound from './pages/NotFound.vue'

const routes = {
  '/': PersonalInformationPage,
  '/access': AccessLevel,
  '/activity-log': ActivityLog,
  '/approval': ApprovalPage,
  '/attendance': AttendancePage,
  '/company': CompanyPage,
  '/dashboard': DashboardPage,
  '/leave': LeavePage,
  '/organization': OrganizationPage,
  '/payroll': PayRollPage,
  '/personal-info': PersonalInformationPage,
  '/portal': PortalPage
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})
</script>

<template>
  <LayoutPage>
    <component :is="currentView" />
  </LayoutPage>
</template>

<style scoped></style>
