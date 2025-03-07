<script setup>
import { ref, shallowReactive } from 'vue'
import { useTransition } from '@vueuse/core'

// The target value we want to transition to
const targetValue = ref(100)
const index = ref(0)
const targetValueList = shallowReactive([
  {
    name: 'foo',
    rate: ref(100)
  },{
    name: 'bar',
    rate: ref(200)
  }
])

targetValueList.forEach((item) => item.animatedRate = useTransition(item.rate, { duration: 1000 }))

// The animated value that smoothly transitions to match `targetValue`
const animatedValue = useTransition(targetValue, {
  duration: 1000, // 1 second animation
})

// Function to change the target value randomly
function updateValue() {
  targetValue.value = Math.floor(Math.random() * 1000)
  targetValueList.forEach(element => {
    element.rate.value = Math.floor(Math.random() * 1000)
  });
}
function addValue() {
  const newVal = {
    'name': `new value ${index.value++}`,
    'rate': ref(0)
  }
  newVal.animatedRate = useTransition(newVal.rate, {duration: 1000})
  targetValueList.push(newVal)
}
</script>

<template>
  <div>
    <button @click="updateValue">Update Value</button>
    <button @click="addValue"> Add value to list</button>
    <p>Target Value: {{ targetValue }}</p>
    <p>Animated Value: {{ Math.round(animatedValue) }}</p>
    <p v-for="val in targetValueList" key="val.name">{{ val.name }} - {{ Math.floor(val.animatedRate.value) }}</p>
  </div>
</template>
