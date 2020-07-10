<template>
  <section class="section">
    <Card :title="payload.title" :img_url="payload.imageUrl" :message="payload.message"/>
  </section>
</template>

<script>
import Card from "../components/Card";

const payloadStore = 'PayloadStore'

export default {
  components: {Card},
  head () {
    return {
      title: 'Your present!...'
    }
  },
  data: function () {
    return {
      payload: {
        title: 'Default title',
        message: 'Default message',
        imageUrl: null
      }
    }
  },
  created() {
    let payload = this.$route.query.payload

    try {
      let storeString = atob(payload)
      localStorage.setItem(payloadStore, storeString)
    } catch (e) {
      // do nothing, fallback to default values
    }
    try {
      this.payload = JSON.parse(localStorage.getItem(payloadStore))
    } catch (e) {
      // do nothing
    }
  },
}
</script>
