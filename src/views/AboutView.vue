<template>
  <div class="game-board">
    <el-card shadow="always">
      <template #header>
        <div class="card-header">
          <span>score: {{ count }}</span>
        </div>
      </template>

    </el-card>

  </div>
</template>

<script setup lang="ts">
import { useCounterStore } from "@/stores/counter"
import { storeToRefs } from "pinia";
import { onMounted, onUnmounted } from "vue"
const counterStore = useCounterStore()
const { count } = storeToRefs(counterStore)

// add event listener
const handleKeyDown = (event: KeyboardEvent) => {
  console.log(event);
};

let target: Element | null = null
onMounted(() => {
  addEventListener("keydown", handleKeyDown);
  target = document.querySelector('.game-board')

})

onUnmounted(() => {
  removeEventListener("keydown", handleKeyDown);
})

const timeChunck = (data:number[], fn:Function, count: number) => {
  const start = () => {
    const arr = data.splice(0, count)
    console.log(arr);
    
    arr.forEach((element: any) => {
      fn(element)
    });
  }
  return function () {
    const t= setInterval(() => {
      if (data.length === 0) {
        return clearInterval(t)
      }
      start()
    },100)
  }
}
const renderDiv = timeChunck(Array.from({ length: 1000 }, (_, i) => i + 1),(n: string) => {
  const div = document.createElement('div')
  div.innerHTML = n  
  target?.appendChild(div)
}, 10)

// renderDiv()

</script>
<style>
@media (min-width: 1024px) {
  .game-board {}
}
</style>
