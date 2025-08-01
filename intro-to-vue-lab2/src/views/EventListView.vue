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
    events.value = null
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
  <h1>Events For Good</h1>

  <!-- Limit Selector -->
  <div class="limit-selector">
    <label for="limit">Events per page:</label>
    <select id="limit" v-model.number="selectedLimit" @change="changeLimit">
      <option :value="2">2</option>
      <option :value="5">5</option>
      <option :value="10">10</option>
    </select>
  </div>

  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <RouterLink
        id="page-prev"
        :to="{ name: 'event-list-view', query: { page: page - 1, limit: limit } }"
        rel="prev"
        v-if="page !== 1"
      >
        &#60; Prev Page
      </RouterLink>

      <RouterLink
        id="page-next"
        :to="{ name: 'event-list-view', query: { page: page + 1, limit: limit } }"
        rel="next"
        v-if="hasNextPage"
      >
        Next Page &#62;
      </RouterLink>
    </div>
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}
.event-wrapper {
  width: 100%;
  max-width: 600px;
}
.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}
#page-prev {
  text-align: left;
}
#page-next {
  text-align: right;
}
.limit-selector {
  margin-bottom: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
  font-size: 1rem;
  width: 100%;
}

</style>
