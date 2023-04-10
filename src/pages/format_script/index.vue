<script setup lang="ts">
import { data } from '~/composables/data'
const DP = 'dp'
const input = ref(data)
const copyIndex = ref(-1)
const newValue = computed(() => {
  if (!input.value)
    return []
  const titleReg = /\*?.*?([0-9]|[A-Z]|[a-z])\n/g
  const scriptReg = /<script\b[^>]*><\/script>/
  const tab = input.value.match(titleReg) as string[]

  if (!tab)
    return []
  const array = input.value.toString().split('\n').filter(Boolean)
  const newData: { title: string; script: string }[] = []
  const typeIndex = tab.map((el) => {
    return array.findIndex(item => item === el.replace('\n', ''))
  })

    ; (function () {
    const len = typeIndex.length
    const lastData = array.slice(typeIndex[len - 1] + 1).join('')
    const headScript = lastData.match(scriptReg)?.[0]
    newData.push({
      title: 'head',
      script: headScript || '',
    },
    {
      title: `${DP}_head`,
      script: headScript || '',
    }, {
      title: array[typeIndex[len - 1]],
      script: lastData.replace(scriptReg, ''),
    },
    )
    typeIndex.reduce((acc, idnex) => {
      newData.push({
        title: array[acc],
        script: array.slice(acc + 1, idnex).join('').replace(scriptReg, ''),
      })
      return acc = idnex
    }, 0)
  }())
  return newData.filter(item => item.script)
})

function formatTile(title: string) {
  return title
}
function copy(script: string, index: number) {
  const aux = document.createElement('input')
  aux.setAttribute('value', script)
  document.body.appendChild(aux)
  aux.select()
  document.execCommand('copy')
  document.body.removeChild(aux)
  copyIndex.value = index
}
</script>

<template>
  <div px-20>
    <h2>输入代码</h2>
    <textarea v-model="input" name="" cols="30" rows="10" p-xy w="100%" border b-rd h-20 />
    <div flex="1" ml1>
      <ul flex="~ wrap" p-10 gap-5>
        <li
          v-for="item, index in newValue" :key="index" w="32%" b b-rd px-5 hover="bg-#CDF0EA/700"
          dark:hover="bg-#111/700" flex="~ col" direction-column
        >
          <h2 text-left font-600 relative mt-6>
            <span
              bg="#fff" dark:color-black border inline-block w-9 h-9 b-rd-7 lh-9 mr-2 font-600 color="#3B78F0"
              text-center style="position: absolute; top: -35px; left: -30px;font-weight:600; font-size: 20px;"
            >{{
              index + 1 }}</span>
            <div color="#3B78F0" text-center style="font-weight:600; font-size: 16px;">
              {{ formatTile(item.title) }}
            </div>
          </h2>
          <p
            line-clamp-3 :title="item.script" color="#666" hover="color-red/700 font-600"
            flex-1
            dark:hover="color-yellow/700 font-600" :class="[index === copyIndex ? 'color-blue' : '']" my-4
          >
            {{ copyIndex === index ? '已复制代码 ✅' : item.script }}
          </p>
          <hr mt-2>

          <div class="footer" mt-2>
            <button>
              <i i-carbon-copy btn @click="copy(item.script, index)" />
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>

<style>
.dark textarea {
  color: aliceblue !important;
  background: #111;
}
</style>
