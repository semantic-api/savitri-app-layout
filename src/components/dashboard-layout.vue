<script setup lang="ts">
import { inject } from 'vue'
import { SvIcon } from '@savitri/ui'
import MainNavbar from '../components/main-navbar.vue'

const menuSchema = inject('menuSchema')
</script>

<template>
  <div class="dashboard">
    <main-navbar :schema="menuSchema"></main-navbar>

    <div class="dashboard__main">
      <div class="dashboard__content">
        <sv-icon
          :name="viewIcon"
          class="dashboard__icon"
        >
          {{ viewTitle }}
        </sv-icon>
        <router-view name="topbar"></router-view>
      </div>
      <div class="dashboard__view">
        <router-view></router-view>
      </div>
    </div>
  </div>
</template>

<style scoped src="./dashboard-layout.scss"></style>
