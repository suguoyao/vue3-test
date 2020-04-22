<template>
  <div class="hello">
    <h1>Welcome to {{ username }}</h1>
    <button @click="inc">Clicked {{ count }} times.</button>
  </div>
</template>

<script>
  import {ref, watchEffect} from 'vue'

  export default {
    name: 'HelloWorld',
    props: {
      username: String
    },
    // The second argument provides a context object which exposes a selective list of properties
    // that were previously exposed on this in 2.x APIs:
    // {attrs, emit, slots} = context
    setup(props, context) {
      // this. // not the `this` you'd expect!
      console.log('setup context', context)

      const count = ref(0)
      const numberRef = ref(1)
      const stringRef = ref('a')

      console.log(numberRef.value)
      numberRef.value++
      console.log(numberRef.value)

      console.log(stringRef) // ouput: {value: "a", _isRef: true}

      watchEffect(() => {
        console.log(`name is: ` + props.username)
      })

      const inc = () => {
        count.value++
      }
      return {
        count,
        inc
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  h3 {
    margin: 40px 0 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
