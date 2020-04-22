<template>
  <div class="hello">
    <h1>{{ username }}!</h1>
    <p>{{ description }}</p>
    <button @click="increment">Clicked {{ count }} times.</button>
    <button @click="showPanelHandle">Show Panel</button>

    <div v-if="showPanel" ref="panelDomRef">Panel content</div>
  </div>
</template>

<script>
  import {
    ref, reactive, watchEffect, computed, readonly,
    onMounted, onUpdated, onUnmounted
  } from 'vue'

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
      const copyCount = readonly(count)
      const numberRef = ref(1)
      const stringRef = ref('a')
      const showPanel = ref(false)
      const panelDomRef = ref(null)

      console.log(numberRef.value) // output: 1
      numberRef.value++
      console.log(numberRef.value) // output: 2
      console.log(stringRef) // output: {value: "a", _isRef: true}

      const state = reactive({
        count
      })

      console.log(state.count) //  output: 0
      state.count = 1
      console.log(count.value) //  output: 1

      // if a new ref is assigned to a property linked to an existing ref
      // it will replace the old ref
      state.count = ref(8)
      console.log(state.count) //  output: 8
      console.log(count.value) //  output: 1

      const isEven = computed(() => count.value % 2 === 0)

      const increment = () => {
        count.value++
        console.log(`Is count an even number ? ${isEven.value}`)

        copyCount.value++ // Warning! Set operation on key "value" failed: target is readonly
      }

      const showPanelHandle = () => {
        showPanel.value = !showPanel.value
      }

      watchEffect(() => {
        console.log(`watchEffect: name is ${props.username}`)
        console.log(count.value)
      })

      /**
       beforeCreate -> use setup()
       created -> use setup()
       beforeMount -> onBeforeMount
       mounted -> onMounted
       beforeUpdate -> onBeforeUpdate
       updated -> onUpdated
       beforeDestroy -> onBeforeUnmount
       destroyed -> onUnmounted
       errorCaptured -> onErrorCaptured
       */
      onMounted(() => {
        console.log('mounted!')
        watchEffect(() => {
          // access the DOM or template refs
        })
      })
      onUpdated(() => {
        console.log('updated!')
        console.log('panelDomRef', panelDomRef.value)
        if (panelDomRef.value) {
          console.log(panelDomRef.value.textContent)
        }
      })
      onUnmounted(() => {
        console.log('unmounted!')
      })

      return {
        count,
        description: ref('This is the description.'),
        showPanel,
        panelDomRef,
        increment,
        showPanelHandle
      }
    },
    // new hooks
    onRenderTracked(e) {
      console.log(e)
    },
    onRenderTriggered(e) {
      console.log(e)
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
