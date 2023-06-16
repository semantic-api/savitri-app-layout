<script setup lang="ts">
import { inject, ref, watch } from 'vue'
import { SvIcon, SvPicture } from '@savitri/ui'
import MainNavbar from '../components/main-navbar.vue'

const menuSchema = inject('menuSchema')
const topbarVisible = ref(false)
const topbarComponent = ref(null)

let topbarObserver: MutationObserver
watch(topbarComponent, () => {
  topbarVisible.value = false
  topbarObserver?.disconnect()

  const topbarElem = document.getElementById('inner-topbar')
  if( !topbarElem ) {
    return
  }

  if( topbarElem.children.length > 0 ) {
    topbarVisible.value = true
  }

  topbarObserver = new MutationObserver(() => {
    if( topbarElem.children.length > 0 ) {
      topbarVisible.value = true
      topbarObserver.disconnect()
    }
  })

  topbarObserver.observe(topbarElem, {
    childList: true
  })

}, { immediate: true })
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

          <div
            v-if="$route.matched.slice(-1)[0].components.topbar"
            id="inner-topbar"
            ref="topbarComponent"
            :style="{
              display: topbarVisible
                ? 'inherit'
                : 'none'
            }"
            class="dashboard__topbar"
          >
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
