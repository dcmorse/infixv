<template>
  <div>
    my conjunction is {{ conjunction }} of type {{ typeof conjunction }}
  </div>
  <select v-model="conjunction">
    <option :value="undefined"></option>
    <option v-for="(operator, i) in operators" :key="i">{{ operator }}</option>
  </select>
  <input
    type="number"
    :value="value"
    @change="(e) => setValue(parseFloat(e.target.value) ?? 0)"
  />
</template>
<script lang="ts">
import { defineComponent, ref } from "vue"

type Operator = "+" | "-" | "*" | "/"
const operators: Operator[] = ["+", "-", "*", "/"]

export default defineComponent({
  name: "Mushroom",
  props: {
    value: Number,
    setValue: Function,
  },
  setup() {
    // const left = ref<number | undefined>(undefined)
    // const right = ref<number | undefined>(undefined)
    // const computedValue = computed(() =>
    //   left.value !== undefined && right.value !== undefined
    //     ? left.value + right.value
    //     : props.value
    // )
    const conjunction = ref<Operator | undefined>(undefined)
    return { conjunction, operators }
  },
})
</script>
