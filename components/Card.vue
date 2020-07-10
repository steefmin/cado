<template>
  <div class="column">
    <div class="card">
      <div class="card-image">
        <figure class="image">
          <img :src="img_url"/>
        </figure>
      </div>
      <header class="card-header">
        <p class="card-header-title has-text-grey">
          {{ titleString }}
        </p>
      </header>
      <div class="card-content">
        {{ messageString }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    message: {
      type: String,
      required: true
    },
    img_url: {
      type: String,
      required: false
    }
  },
  computed: {
    titleString() {
      return this.title === '' ? 'Preview' : this.title
    },
    messageString() {
      return this.message === '' ? 'this will show you the gift message that you\'ll be sending' : this.message
    }
  }
}
</script>
