<script setup lang="ts">
import { inject, ref, watch } from 'vue'
import { SvIcon } from '@savitri/ui'
import MainNavbar from '../components/main-navbar.vue'

const menuSchema = inject('menuSchema')
const topbarComponent = ref(null)
const topbarVisible = ref(false)

watch(topbarComponent, () => {
  topbarVisible.value = false

  const topbarElem = document.getElementById('inner-topbar')
  const mo = new MutationObserver(() => {
    if( topbarElem.children.length > 0 ) {
      topbarVisible.value = true
      mo.disconnect()
    }
  })

  mo.observe(topbarElem, {
    childList: true
  })
})
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
          <div
            id="inner-topbar"
            :style="{
              display: topbarVisible
                ? 'inherit'
                : 'none'
            }"
            class="dashboard__view-topbar"
          >
            <router-view name="topbar" v-slot="{ Component }">
              <component
                :is="Component"
                ref="topbarComponent"
              ></component>
            </router-view>
          </div>

          <transition name="fade" mode="out-in">
            <div
              :key="$route.fullPath"
              class="dashboard__view-content"
            >
              <router-view></router-view>
            </div>
          </transition>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped src="./dashboard-layout.scss"></style>

<style lang="scss">
.fade-enter-active,
.fade-leave-active {
  width: 100%;
  transition: opacity .19s ease-in;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
