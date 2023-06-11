<script setup lang="ts">
import type { MenuSchema } from '@savitri/web'
import { reactive, toRefs, onMounted } from 'vue'
import { useStore, useRouter, useNavbar } from '@savitri/web'
import { SvIcon, SvPicture } from '@savitri/ui'

type Props = {
  schema: MenuSchema
}

const props = defineProps<Props>()
const navbarRefs = reactive({
  routesWithChildren: [],
  isCurrent: (..._args: Parameters<Awaited<ReturnType<typeof useNavbar>>['isCurrent']>) => false
})

const {
  routesWithChildren,
  isCurrent

} = toRefs(navbarRefs)

const router = await useRouter()
const push = (...args: Parameters<typeof router.push>) => {
  window.scrollTo(0, 0)
  router.push(...args)
}

onMounted(async () => {
  useStore('user').functions.ping(null, {
    skipLoading: true
  })

  const navbar = await useNavbar(props)
  Object.assign(navbarRefs, navbar)
})
</script>

<template>
  <aside class="navbar no-print">
    <img
      v-clickable
      class="navbar__logo"
      src="/static/logo.png"
      @click="push('/dashboard')"
    />
    <nav class="navbar__entries">
      <div
        v-once
        v-for="(entry, index) in routesWithChildren"
        :key="`entry-${index}`"
      >
        <div class="navbar__entry-title">
          {{ entry.meta.title }}
        </div>

        <div>
          <div
            v-clickable
            v-for="route in entry.children"
            :key="route.name"
            :class="`
              navbar__route
              ${isCurrent(route) && 'navbar__route--current'}
            `"

            :title="$tc(route.meta.title || 'untitled', 2)"
            @click="push({ name: route.name })"
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
    </nav>
  </aside>
</template>

<style scoped src="./main-navbar.scss"></style>
