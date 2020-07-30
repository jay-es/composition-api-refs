<template>
  <img ref="imgRef" alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld ref="componentRef" msg="Hello Vue 3.0 + Vite" />
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import HelloWorld from './components/HelloWorld.vue'

export default defineComponent({
  name: 'App',
  components: {
    HelloWorld,
  },
  setup() {
    const imgRef = ref<HTMLImageElement>()

    console.log(imgRef.value) // -> undefined

    onMounted(() => {
      console.log(imgRef.value) // <img>
      console.log(imgRef.value?.clientHeight) // -> 0

      imgRef.value?.addEventListener('load', () => {
        console.log(imgRef.value?.clientHeight) // -> 200
      })
    })

    const componentRef = ref<InstanceType<typeof HelloWorld>>()

    console.log(componentRef.value) // undefined

    onMounted(() => {
      console.log(componentRef.value) // コンポーネントの Proxy

      if (!componentRef.value) return

      console.log(componentRef.value.$el) // テキストノード

      console.log(componentRef.value.$props.msg) // props で渡した値
      console.log(componentRef.value.$data.count) // コンポーネントの $data の値

      componentRef.value.count++ // バッドプラクティス！
      console.log(componentRef.value.count) // 増えている
    })

    return { imgRef, componentRef }
  },
})
</script>
