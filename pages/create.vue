<template>
  <section class="section">
    <h2 class="title is-2">Create a personal gift message</h2>

    <section class="hero is-info">

      <div class="hero-body">
        <h4 class="subtitle is-4">Preview</h4>
        <Card :title="payload.title" :img_url="imageUrl()" :message="payload.message"/>
      </div>
    </section>
    <b-field label="Title">
      <b-input
        type="text"
        required
        placeholder="A bold title for your gift"
        v-model="payload.title"
      >
      </b-input>
    </b-field>
    <b-field label="Image URL">
      <b-input
        type="url"
        ref="urlInput"
        placeholder="https://some.image/url.png"
        v-model="payload.imageUrl"
      >
      </b-input>
    </b-field>
    <b-field label="Message">
        <b-input
          type="textarea"
          placeholder="Your personal message"
          v-model="payload.message"
          required
        />
    </b-field>
    <div class="buttons">
      <b-button type="is-success" icon-left="check" @click="save()">Save</b-button>
      <b-button type="is-info" icon-left="share" @click="isLinkActive = true" :disabled="isLinkButtonDisabled">Share link</b-button>
      <b-button type="is-info" icon-left="qrcode" @click="save(); isQrCodeActive = true" :disabled="isQrCodeButtonDisabled">Create QR-code</b-button>
      <b-button type="is-danger" icon-left="redo" @click="clear()">Clear</b-button>
    </div>
    <b-message auto-close title="Link" type="is-success" has-icon :active.sync="isLinkActive"
               aria-close-label="Close message" :duration=10000
    >
      <a :href="getPayloadLink()" target="_blank">{{ getPayloadLink() }}</a>
    </b-message>
    <b-message title="QR-Code" type="is-success" :active.sync="isQrCodeActive"
               aria-close-label="Close message"
    >
      <nuxt-link to="viewQR">
        <qrcode-vue :value="getPayloadLink()" :size="size()" level="H"></qrcode-vue>
        Click the QRcode to view in a separate page
      </nuxt-link>

    </b-message>
  </section>
</template>

<script>
  import Card from '~/components/Card'
  import QrcodeVue from 'qrcode.vue'

  const payloadStore = 'PayloadStore'

  export default {
    components: {
      Card,
      QrcodeVue
    },
    data() {
      return {
        isLinkActive: false,
        isQrCodeActive: false,
        payload: {
          title: '',
          message: '',
          imageUrl: null
        }
      }
    },
    head() {
      return {
        title: 'A gift, for you!'
      }
    },
    computed: {
      isLinkButtonDisabled() {
        if (this.isLinkActive) {
          return true
        }
        if (this.payloadIsDefault()) {
          return true
        }
        return false
      },
      isQrCodeButtonDisabled() {
        if (this.isQrCodeActive) {
          return true
        }
        if (this.payloadIsDefault()) {
          return true
        }
        return false
      },
    },
    created() {
      let payload = this.$route.query.payload

      try {
        let storeString = atob(payload)
        localStorage.setItem(payloadStore, storeString)
      } catch (e) {
        // do nothing, fallback to default values
      }
      let storeString = localStorage.getItem(payloadStore)
      if (storeString) {
        try {
          this.payload = JSON.parse(storeString)
        } catch (e) {
          this.payload = {
            title: '',
            message: '',
            imageUrl: null
          }
        }
      }
    },
    methods: {
      payloadIsDefault() {
        if (this.payload.title === '') {
          return true
        }
        if (this.payload.message === '') {
          return true
        }
        return false
      },
      query() {
        const payload = JSON.stringify(this.payload)
        return `?payload=${btoa(payload)}`
      },
      getPayloadLink() {
        return `${window.location.origin}/${this.query()}`
      },
      size() {
        return window.innerWidth * 0.2
      },
      save() {
        localStorage.setItem(payloadStore, JSON.stringify(this.payload))
        this.$buefy.toast.open({message: 'Saved', type: 'is-success'})
      },
      clear() {
        localStorage.removeItem(payloadStore)
        this.payload = {
          title: '',
          message: '',
          imageUrl: null
        }
        this.$buefy.toast.open({message: 'Form cleared', type: 'is-success'})
      },
      imageUrl() {
        if (this.$refs.urlInput) {
          if (false === this.$refs.urlInput.checkHtml5Validity()) {
            return null
          }

          return this.payload.imageUrl
        }
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
</style>
