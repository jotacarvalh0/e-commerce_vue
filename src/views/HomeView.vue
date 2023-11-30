<template>
  <div class="home">
    <div class="banner pt-32">
      <div class="container">
        <img src="images/banner.webp" alt="Banner">
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  }
}
</script>
