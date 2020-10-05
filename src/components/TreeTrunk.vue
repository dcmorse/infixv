<template>
  <TreeNode :value="value" :setValue="setValue" />
</template>
<script lang="ts">
import { defineComponent, ref } from "vue"
import TreeNode from "./TreeNode.vue"

export default defineComponent({
  name: "TreeTrunk",
  setup() {
    const value = ref<number>(100)
    const setValue = (n: number) => {
      value.value = n
    }
    return { value, setValue }
  },
  components: { TreeNode },
})
</script>
