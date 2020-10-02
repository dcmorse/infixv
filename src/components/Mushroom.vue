<template>
  <div class="node">
    <div v-if="isAtom" class="node">
      {{ value }}
      <input
        type="number"
        :value="value"
        @change="(e) => setValue(parseFloat(e.target.value) ?? 0)"
      />
      <button @click.stop="toggleAtomic">
        <span v-if="isAtom">←◉→</span>
        <span v-else>→○←</span>
      </button>
    </div>
    <div v-else>
      <div class="row">
        <Mushroom :value="leftValue" :setValue="setLeftValue" />
        <div class="node">
          {{ value }}
          <select v-model="conjunction">
            <option :value="undefined"></option>
            <option v-for="(operator, i) in operators" :key="i">{{
              operator
            }}</option>
          </select>
          <button @click.stop="toggleAtomic">
            <span v-if="isAtom">←◉→</span>
            <span v-else>→○←</span>
          </button>
        </div>
        <Mushroom :value="rightValue" :setValue="setRightValue" />
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue"

type Operator = "+" | "-" | "*" | "/"
const operators: Operator[] = ["+", "-", "*", "/"]

export default defineComponent({
  name: "Mushroom",
  props: {
    value: { type: Number, required: true },
    setValue: { type: Function, required: true },
  },
  setup(props) {
    const isAtom = ref<boolean>(true)
    const toggleAtomic = () => {
      isAtom.value = !isAtom.value
    }
    const leftValue = ref<number>(props.value)
    const rightValue = ref<number>(0)
    const setLeftValue = (n: number) => {
      console.log("hoa")
      leftValue.value = n
    }
    const setRightValue = (n: number) => {
      rightValue.value = n
    }
    const conjunction = ref<Operator | undefined>(undefined)
    return {
      conjunction,
      operators,
      isAtom,
      toggleAtomic,
      leftValue,
      setLeftValue,
      rightValue,
      setRightValue,
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
