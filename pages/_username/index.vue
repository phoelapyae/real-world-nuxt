<template>
  <div>
    <h1>Event List</h1>

    <p v-if="$fetchState.pending">Fetching mountains...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>

    <EventCard
      v-for="(event, index) in events"
      :key="index"
      :event="event"
      :data-index="index"
    />

  </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue';
export default {
  name: 'IndexPage',
  components: {
    EventCard
  },
  head() {
    return {
      title: 'Event Listing'
    }
  },
  data() {
    return {
      events: []
    }
  },
  activated() {
    // Call fetch again if last fetch more than 30 sec ago
    if (this.$fetchState.timestamp <= Date.now() - 30000) {
      this.$fetch()
    }
  },
  async fetch() {
    this.events = await fetch('http://localhost:3000/events').then(res =>
      res.json()
    )
  }
}
</script>
