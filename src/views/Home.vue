<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld :username="username"/>
  </div>
</template>

<script>
  // @ is an alias to /src
  import HelloWorld from '@/components/HelloWorld.vue'

  import {ref} from 'vue'

  export default {
    name: 'Home',
    components: {
      HelloWorld
    },
    setup() {
      const username = ref('SUGAR')

      setTimeout(() => {
        // Will trigger the watchEffect method of HelloWorld component
        username.value = "SSSSS"
      }, 1000)

      return {
        username
      }
    }
  }
</script>
