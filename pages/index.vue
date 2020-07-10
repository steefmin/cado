<template>
  <div class="section">
    <nuxt-link :to="linkTo"><Present/></nuxt-link>
    <h1 class="title" v-if="linkToCreate">Click to create a gift!</h1>
    <h1 class="title" v-if="!linkToCreate">Click to open your gift!</h1>
  </div>
</template>

<script>
import Card from '~/components/Card'
import Present from "../components/Present"

const payloadStore = 'PayloadStore'

export default {
  components: {
    Present,
    Card
  },
  head() {
    return {
      title: 'A gift, for you!'
    }
  },
  data() {
    return {
      linkToCreate: true
    }
  },
  computed: {
    linkTo() {
      if (this.linkToCreate) {
        return 'create'
      }
      return 'show'
    }
  },
  created() {
    let payload = this.$route.query.payload

    try {
      let storeString = atob(payload)
      localStorage.setItem(payloadStore, storeString)
      this.linkToCreate = false
    } catch (e) {
      // do nothing, fallback to default values
    }
    if (localStorage.getItem(payloadStore)) {
      this.linkToCreate = false
    }
  }
}
</script>


<style scoped>
  .section {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  h1 {
    text-align: center;
  }
</style>
