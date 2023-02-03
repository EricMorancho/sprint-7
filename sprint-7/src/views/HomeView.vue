<template>
  <div class="home">
    <Home />
  </div>
</template>

<script default>
// @ is an alias to /src
import Home from '@/components/Home.vue'

export default {
  name: 'HomeView',
  components: {
    Home
  }
}
</script>
