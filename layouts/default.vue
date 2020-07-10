<template>
  <div>
    <Navbar/>
    <div class="columns">
      <div class="column"></div>
      <div class="column is-one-third-fullhd is-two-fifths-widescreen is-half-desktop is-full-tablet">
        <section class="main-content section is-medium">
          <nuxt/>
        </section>
      </div>
      <div class="column"></div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
export default {
  components: {Navbar}
}
</script>
