<template>
  <div>
    <div>Create an Event</div>
    <form @submit.prevent="createEvent">
      <label>Select a category</label>
      <select v-model="event.category">
        <option v-for="cat in categories" :key="cat">{{ cat }}</option>
      </select>

      <h3>Name & describe your event</h3>
      <BaseInput
        type="text"
        placeholder="Add a title"
        class="field"
        label="Title"
        v-model="event.title"
      />
      <BaseInput
        type="text"
        placeholder="Add a description"
        class="field"
        label="Description"
        v-model="event.description"
      />

      <h3>Where is your event?</h3>
      <BaseInput
        type="text"
        placeholder="Add a location"
        class="field"
        label="Location"
        v-model="event.location"
      />

      <h3>When is your event?</h3>

      <div class="field">
        <label>Date</label>
        <datepicker v-model="event.date" placeholder="Select a date" />
      </div>

      <div class="field">
        <label>Select a time</label>
        <select v-model="event.time">
          <option v-for="time in times" :key="time">{{ time }}</option>
        </select>
      </div>

      <input type="submit" class="button -fill-gradient" value="Submit" />
    </form>
  </div>
</template>

<script>
// import { mapState, mapGetters } from 'vuex'
import NProgress from 'nprogress'
import Datepicker from 'vuejs-datepicker'

export default {
  components: {
    Datepicker
  },
  data() {
    const times = []
    for (let i = 1; i <= 24; i++) {
      times.push(i + ':00')
    }
    return {
      times,
      categories: this.$store.state.categories,
      event: this.createFreshEventObject()
    }
  },
  methods: {
    createEvent() {
      NProgress.start()
      this.$store
        .dispatch('event/createEvent', this.event)
        .then(() => {
          this.$router.push({
            name: 'event-show',
            params: { id: this.event.id }
          })
          this.event = this.createFreshEventObject()
        })
        .catch(() => {
          NProgress.dode()
        })
    },
    createFreshEventObject() {
      const user = this.$store.state.user.user
      const id = Math.floor(Math.random() * 100000)

      return {
        id: id,
        user: user,
        category: '',
        organizer: user,
        titie: '',
        description: '',
        location: '',
        date: '',
        time: '',
        attendees: []
      }
    }
  }
  // computed: mapState({
  //   user: 'user',
  //   categories: 'categories'
  // })d
  // computed: {
  //   ...mapState(['user', 'categories'])
  // }
}
</script>

<style lang="scss" scoped>
.field {
  margin-bottom: 24px;
}
</style>
