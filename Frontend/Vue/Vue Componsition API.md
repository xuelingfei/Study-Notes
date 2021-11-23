
```vue
<template></template>
<script>
import {
  ref,
  reactive,
  toRaw,
  computed,
  watch,
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onErrorCaptured,
  onRenderTracked,
  onRenderTriggered,
  onActivated,
  onDeactivated,
} from "vue"
import { useStore } from "vuex"
import { useRoute, useRouter } from "vue-router"

export default {
  name: "",
  mixins: [],
  components: {},
  setup() {
    const store = useStore()
    const route = useRoute()
    const router = useRouter()
    const a = computed(() => store.state.a.id)

    watch(a, (newValue) => {})
    watch(
      () => route.name,
      (name) => {}
    )

    onMounted(() => {})

    return {}
  },
}
</script>
<style lang="scss" scoped>
@import "./index.scss";
</style>
```