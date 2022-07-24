<template>
  <h1>Events for Good</h1>

  <!-- Modify EventList.vue to pass on page number to EventService API -->
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <router-link
      :to="{ name: 'EventList', query: { page: page - 1 } }"
      rel="prev"
      v-if="page != 1"
      >Prev Page</router-link
    >

    <router-link
      :to="{ name: 'EventList', query: { page: page + 1 } }"
      rel="next"
      >Next Page</router-link
    >
  </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import { watchEffect } from 'vue';

export default {
  name: 'EventList',
  props: ['page'],
  components: {
    EventCard
  },
  data() {
    return {
      events: null
    }
  },
  created() {
    this.events = null; // Clear out the events on the page, so our user knows the API has been called
    // When reactive object that are accessed inside this function change, run this function again
    watchEffect(() => {
    EventService.getEvents(2, this.page) // 2 : Events per page / 'this.page' : Send in the current page
      .then(response => {
        this.events = response.data;
      })
      .catch(error => {
        console.log(error);
      })
      })
  }
}
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
