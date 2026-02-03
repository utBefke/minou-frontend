<script setup>
import { ref, onMounted } from 'vue'
import { usePageNavigation } from '@/composables/usePageNavigation'
import { useSwipeHandler } from '@/composables/gestures/useSwipeHandler'

const viewport = ref(null)
const currentPageEl = ref(null)
const nextPageEl = ref(null)
const prevPageEl = ref(null)

const { currentPage, nextPage, prevPage } = usePageNavigation()

onMounted(() => {
  console.log('[Viewport] onMounted - refs:', {
    viewport: viewport.value,
    currentPage: currentPageEl.value,
    nextPage: nextPageEl.value,
    prevPage: prevPageEl.value
  })

  // Use nextTick to ensure refs are fully ready
  setTimeout(() => {
    console.log('[Viewport] After timeout - refs:', {
      viewport: viewport.value,
      currentPage: currentPageEl.value,
      nextPage: nextPageEl.value,
      prevPage: prevPageEl.value
    })

    useSwipeHandler(viewport, {
      currentPageEl,
      nextPageEl,
      prevPageEl
    })
  }, 100)
})
</script>

<template>
  <div ref="viewport" class="viewport">
    <div v-if="prevPage" ref="prevPageEl" class="page-slot prev">
      <div class="page-content">
        <component :is="prevPage.component" />
      </div>
    </div>

    <div ref="currentPageEl" class="page-slot current">
      <div class="page-content">
        <component :is="currentPage.component" />
      </div>
    </div>

    <div v-if="nextPage" ref="nextPageEl" class="page-slot next">
      <div class="page-content">
        <component :is="nextPage.component" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.viewport {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  touch-action: none;
  background: #161412;
}

.page-slot {
  position: absolute;
  inset: 0;
  backface-visibility: hidden;
}

.page-content {
  width: 100%;
  height: 100%;
  transform-origin: center;
}

/* Initial states - animations will control these */
.page-slot.current {
  z-index: 2;
  opacity: 1;
}

.page-slot.prev,
.page-slot.next {
  z-index: 1;
  opacity: 0;
}
</style>
