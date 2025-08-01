<script setup lang="ts">
import { toRefs } from 'vue'
import { type Event } from '@/types'
import { useMessageStore } from '@/stores/message'
import { storeToRefs } from 'pinia'
import { useRouter } from 'vue-router'

const store = useMessageStore()
const { message } = storeToRefs(store)
const props = defineProps<{
  event: Event
  id: String
}>()

// eslint-disable-next-line @typescript-eslint/no-unused-vars
const { event } = toRefs(props)
const router = useRouter()
const edit = () => {
  store.updateMessage('You are successuflly edited for ' + props.event.title)
  setTimeout(() => {
  store.resetMessage()
}, 3000)
router.push({ name: 'event-detail-view', params: { id: props.event.id } })
}
</script>

<template>
  <p>Edit event here</p>
  <button @click="edit">Edit</button>
</template>
