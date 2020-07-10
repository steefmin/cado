<template>
  <figure class="image">
    <qrcode-vue :value="getPayloadLink()" :size="size()" level="H"></qrcode-vue>
  </figure>
</template>

<script>
import QrcodeVue from 'qrcode.vue'

const payloadStore = 'PayloadStore'

export default {
  name: "viewQR",
  components: {
    QrcodeVue
  },
  data() {
    return {
      payload: {}
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
  methods: {
    route() {
      const payload = JSON.stringify(this.payload)
      return `?payload=${btoa(payload)}`
    },
    getPayloadLink() {
      return `${window.location.origin}/${this.route()}`
    },
    size() {
      return window.innerWidth * 0.3
    }
  }
}
</script>

<style scoped>
.image {
  display: flex;
  justify-content: center;
}
</style>
