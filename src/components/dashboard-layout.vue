<script setup lang="ts">
import { inject } from 'vue'
import { SvIcon } from '@savitri/ui'
import MainNavbar from '../components/main-navbar.vue'

const menuSchema = inject('menuSchema')
</script>

<template>
  <div class="dashboard">
    <main-navbar :schema="menuSchema"></main-navbar>

    <div style="position: relative; width: 100%">
      <transition name="fade">
        <div class="dashboard__main" :key="$route.fullPath">
          <div class="dashboard__content">
            <sv-icon
              :name="viewIcon"
              class="dashboard__view-title"
            >
              {{ viewTitle }}
            </sv-icon>
            <router-view name="topbar"></router-view>
          </div>
          <div class="dashboard__view">
            <router-view></router-view>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<style scoped src="./dashboard-layout.scss"></style>

<style lang="scss">
.fade-enter-active,
.fade-leave-active {
  position: absolute;
  width: 100%;
  transition: opacity .35s ease-in-out;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
