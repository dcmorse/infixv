<template>
  <div class="node">
    <div v-if="isAtom" class="node">
      {{ value }}
      <input type="number" :value="value" @change="handleChange" />
      <button @click.stop="toggleAtomic">
        <span v-if="isAtom">←◉→</span>
        <span v-else>→○←</span>
      </button>
    </div>
    <div v-else>
      <div class="row">
        <TreeNode :value="leftValue" :setValue="setLeftValue" />
        <div class="node">
          {{ value }}
          <select v-model="conjunction" @change="bubbleValue">
            <option v-for="(operator, i) in operators" :key="i">{{
              operator
            }}</option>
          </select>
          <button @click.stop="toggleAtomic">
            <span v-if="isAtom">←◉→</span>
            <span v-else>→○←</span>
          </button>
        </div>
        <TreeNode :value="rightValue" :setValue="setRightValue" />
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue"

type Operator = "+" | "-" | "*" | "/"
const operators: Operator[] = ["+", "-", "*", "/"]

export default defineComponent({
  name: "TreeNode",
  props: {
    value: { type: Number, required: true },
    setValue: { type: Function, required: true },
  },
  setup(props) {
    const isAtom = ref<boolean>(true)
    const toggleAtomic = () => {
      isAtom.value = !isAtom.value
    }
    const conjunction = ref<Operator>("+")
    const leftValue = ref<number>(props.value)
    const rightValue = ref<number>(0)
    const handleChange = (event: any) => {
      const value = parseFloat(event.target.value) ?? 0
      props.setValue(value)
      conjunction.value = "+"
      leftValue.value = value
      rightValue.value = 0
    }
    const bubbleValue = () => {
      const l = leftValue.value
      const r = rightValue.value
      const set = (n: number) => props.setValue(n)
      switch (conjunction.value) {
        case "+":
          return set(l + r)
        case "-":
          return set(l - r)
        case "*":
          return set(l * r)
        case "/":
          return set(l / r)
      }
    }
    const setLeftValue = (n: number) => {
      leftValue.value = n
      bubbleValue()
    }
    const setRightValue = (n: number) => {
      rightValue.value = n
      bubbleValue()
    }
    return {
      conjunction,
      operators,
      isAtom,
      toggleAtomic,
      leftValue,
      setLeftValue,
      rightValue,
      setRightValue,
      handleChange,
      bubbleValue,
    }
  },
})
</script>
<style scoped>
.node {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.row {
  display: flex;
}
.row :nth-child(odd) {
  margin-top: 1em;
}
button,
select,
input {
  width: 60px;
}
</style>
