<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import { type Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import EventService from '@/services/EventService'

const route = useRoute()
const router = useRouter()

const events = ref<Event[] | null>(null)
const totalEvents = ref(0)

const page = computed(() => Number(route.query.page) || 1)
const limit = computed(() => Number(route.query.limit) || 2)

const selectedLimit = ref(limit.value)

const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / limit.value)
  return page.value < totalPages
})

onMounted(() => {
  watchEffect(() => {
    EventService.getEvents(limit.value, page.value)
      .then((response) => {
        events.value = response.data
        totalEvents.value = parseInt(response.headers['x-total-count'])
      })
      .catch((error) => {
        console.error('There was an error!', error)
      })
  })
})

const changeLimit = () => {
  router.push({
    name: 'event-list-view',
    query: { page: 1, limit: selectedLimit.value }
  })
}
</script>

<template>
  <h1 class="text-2xl font-bold">Events For Good</h1>

  <!-- Limit Selector -->
  <div class="mb-4 flex justify-center items-center gap-2 text-base w-full">
    <label for="limit">Events per page:</label>
    <select
      id="limit"
      v-model.number="selectedLimit"
      @change="changeLimit"
      class="border border-gray-300 rounded px-2 py-1"
    >
      <option :value="2">2</option>
      <option :value="5">5</option>
      <option :value="10">10</option>
    </select>
  </div>

  <div class="w-full max-w-xl mx-auto flex flex-col gap-6">
  <EventCard
    v-for="event in events"
    :key="event.id"
    :event="event"
  />

  <div class="flex w-[290px] mx-auto">
    <RouterLink
      id="page-prev"
      :to="{ name: 'event-list-view', query: { page: page - 1, limit: limit } }"
      rel="prev"
      v-if="page !== 1"
      class="flex-1 text-left text-[#2c3e50] no-underline"
    >
      &#60; Prev Page
    </RouterLink>

    <RouterLink
      id="page-next"
      :to="{ name: 'event-list-view', query: { page: page + 1, limit: limit } }"
      rel="next"
      v-if="hasNextPage"
      class="flex-1 text-right text-[#2c3e50] no-underline"
    >
      Next Page &#62;
    </RouterLink>
  </div>
</div>

</template>
