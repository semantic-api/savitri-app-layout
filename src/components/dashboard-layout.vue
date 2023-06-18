<script setup lang="ts">
import { inject } from 'vue'
import { SvIcon, SvPicture } from '@savitri/ui'
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
            class="dashboard__super-title"
          >
            {{ viewTitle }}
          </sv-icon>

          <div class="dashboard__topbar">
            <router-view name="topbar"></router-view>
          </div>

          <slot
            v-if="$slots.super"
            name="super"
          ></slot>

          <div
            v-clickable
            class="dashboard__user"
            @click="$router.push('/dashboard/user/profile')"
          >
            <sv-picture
              :url="currentUser.picture?.link"
              class="dashboard__user-picture"
            ></sv-picture>
            <div>
              {{ currentUser.first_name }}
            </div>
          </div>
        </div>

        <div class="dashboard__view">
          <router-view v-slot="{ Component }">
            <component :is="Component">
              <template
                v-for="slotName in Object.keys($slots)"
                v-slot:[slotName]
              >
                <slot :name="slotName"></slot>
              </template>
            </component>
          </router-view>
        </div>

      </div>

    </div>
  </div>
</template>

<style scoped src="./dashboard-layout.scss"></style>
