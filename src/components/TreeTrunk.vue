<template>
  <TreeNode :value="value" :setValue="setValue" :isRoot="true" />
  <div class="equals">
    equals<br />
    <div class="total">{{ value }}</div>
  </div>
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

<style scoped>
.equals {
  margin-top: 20px;
  font-size: 200%;
  text-align: center;
}

.total {
  margin-top: 20px;
  font-size: 200%;
}
</style>
