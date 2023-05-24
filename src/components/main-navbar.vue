<script setup lang="ts">
import type { MenuSchema } from '@savitri/web'
import { useNavbar } from '@savitri/web'
import { SvIcon, SvPicture } from '@savitri/ui'

type Props = {
  schema: MenuSchema
}

const props = defineProps<Props>()

const {
  routesWithChildren,
  isCurrent

} = await useNavbar(props)
</script>

<template>
  <div class="navbar">
    <img
      v-clickable
      class="navbar__logo"
      src="/static/logo.png"
      @click="$router.push('/dashboard')"
    />
    <div class="navbar__routes">
      <div
        v-for="(entry, index) in routesWithChildren"
        :key="`entry-${index}`"
        class="navbar-entry"
      >
        <div
          v-clickable
          v-for="route in entry.children"
          :key="route.name"
          :class="`
            navbar__route
            ${isCurrent(route) && 'navbar__route--current'}
          `"

          :title="$tc(route.meta.title || 'untitled', 2)"
          @click="$router.push({ name: route.name })"
        >
          <sv-icon :name="route.meta?.icon || 'file'"></sv-icon>
          <div>
            <span>{{ $tc(route.meta.title || 'untitled', 2) }}</span>
            <span v-if="route.badgeFunction">
              ({{
                useStore(route.badgeFunction.split('@')[0])
                  .customGetter[route.badgeFunction.split('@')[1]]('navbar', route.badgePayload)
              }})
            </span>
          </div>
        </div>
      </div>
    </div>

    <div
      v-clickable
      class="navbar__picture-container"
      @click="$router.push('/dashboard/user/profile')"
    >
      <sv-picture
        :url="currentUser.picture?.link"
        class="picture"
      ></sv-picture>
    </div>
  </div>
</template>

<style scoped src="./main-navbar.scss"></style>
