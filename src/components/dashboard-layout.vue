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
      <div class="dashboard__main">
        <div class="dashboard__super">
          <sv-icon
            :name="viewIcon"
            class="dashboard__view-title"
          >
            {{ viewTitle }}
          </sv-icon>
          <div class="dashboard__user">
            <sv-picture
              :url="currentUser.picture?.link"
              class="dashboard__user-picture"
            ></sv-picture>
            <div>
              {{ currentUser.first_name }}
            </div>
          </div>
        </div>

        <transition name="fade" mode="out-in">
          <div class="dashboard__view" :key="$route.fullPath">
            <div v-if="$route.matched.slice(-1)[0].components.topbar" class="dashboard__topbar">
              <router-view name="topbar"></router-view>
            </div>
            <router-view></router-view>
          </div>
        </transition>
      </div>

    </div>
  </div>
</template>

<style scoped src="./dashboard-layout.scss"></style>

<style lang="scss">
.fade-enter-active,
.fade-leave-active {
  position: absolute;
  width: 100%;
  transition: opacity .17s ease-in;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
