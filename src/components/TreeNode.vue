<template>
  <div class="column">
    <div v-if="isAtom" class="column">
      {{ value }}
      <input type="number" :value="value" @change="handleChange" />
      <button @click.stop="toggleAtomic">
        <span v-if="isAtom">←◉→</span>
        <span v-else>→○←</span>
      </button>
    </div>
    <div v-else>
      <div class="row">
        <span v-if="!isRoot">(</span>
        <TreeNode :value="leftValue" :setValue="setLeftValue" />
        <div class="column">
          {{ conjunction }}
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
        <span v-if="!isRoot">)</span>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue"

type Operator = "+" | "-" | "*" | "/"
const operators: Operator[] = ["+", "-", "*", "/"]

const parseNumberFromEvent = (event: Event) => {
  if (event.target) {
    const target = event.target as { value?: string }
    return parseFloat(target.value || "0") ?? 0
  } else {
    return 0
  }
}
export default defineComponent({
  name: "TreeNode",
  props: {
    value: { type: Number, required: true },
    setValue: { type: Function, required: true },
    isRoot: { type: Boolean, required: false, default: false },
  },
  setup(props) {
    const isAtom = ref<boolean>(true)
    const toggleAtomic = () => {
      isAtom.value = !isAtom.value
    }
    const conjunction = ref<Operator>("+")
    const leftValue = ref<number>(props.value)
    const rightValue = ref<number>(0)
    const handleChange = (event: Event) => {
      const value = parseNumberFromEvent(event)
      const target = props.setValue(value)
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
.column {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.row {
  display: flex;
}
select,
input {
  width: 60px;
  height: 30px;
  box-sizing: border-box;
}
button {
  font-size: smaller;
  width: 60px;
  height: 30px;
  background: #0069ed;
  color: #ffffff;
}
</style>
