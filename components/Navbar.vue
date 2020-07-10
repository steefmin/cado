<template>
  <div>
    <b-navbar>
      <template slot="brand">
      </template>
      <template slot="end">
        <b-navbar-item tag="nuxt-link" to="/">
          Home
        </b-navbar-item>
        <b-navbar-item tag="nuxt-link" to="/create">
          Create your own
        </b-navbar-item>
      </template>
    </b-navbar>
  </div>
</template>

<script>
    export default {
        name: "Navbar"
    }
</script>

<style scoped>

</style>
