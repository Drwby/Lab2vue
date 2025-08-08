<script setup lang="ts">
import  { type Event } from '@/types'
defineProps<{
  event: Event
}>()

// const event = ref({
//   id: 5928101,
//   category: 'animal welfare',
//   title: 'Cat Adoption Day',
//   description: 'Find your new feline friend at this event.',
//   location: 'Meow Town',
//   date: 'January 28, 2022',
//   time: '12:00',
//   petsAllowed: true,
//   organizer: 'Kat Laydee'
// })

</script>

<template>
  <RouterLink
    :to="{ name: 'event-detail-view', params: { id: event.id } }"
    class="no-underline text-[#2c3e50]"
  >
    <div
      class="cursor-pointer border border-[#39495c] p-5 w-[250px] mb-[18px] transition-transform duration-200 hover:scale-[1.01] hover:shadow-[0_3px_12px_rgba(0,0,0,0.2)]"
    >
      <h2>{{ event.title }}</h2>
      <span>{{ event.time }} on {{ event.date }}</span>
    </div>
  </RouterLink>
</template>

