<script setup lang="ts">
import { JsonViewer } from 'vue3-json-viewer'
import 'vue3-json-viewer/dist/index.css'
const input = ref<string>('')
const output = computed(() => {
  if (!input.value)
    return ''
  const dataArray = input.value
    .trim()
    .replaceAll('.com', '')
    .replaceAll('.app', '')
    .replaceAll('.cn', '')
    .replaceAll('.top', '')
    .replaceAll('.site', '')
    .replaceAll('.cn', '')
    .split('\n')
    .filter(Boolean)
  const includeChineseCharacters: number[] = []
  const returnValue = []
  function format(index = 0): void {
    const reg = /.*[\u4E00-\u9FA5].*/
    const hz: number = dataArray.slice(index).findIndex((item) => {
      return reg.test(item)
    })
    if (hz !== -1) {
      includeChineseCharacters.push(hz + index)
      format(hz + 1 + index)
    }
  }
  format()
  const len: number = includeChineseCharacters.length
  includeChineseCharacters.reduce((pre, next) => {
    if (next || len === 1) {
      returnValue.push({
        name: dataArray[pre],
        data: dataArray.slice(pre + 1, len === 1 ? Infinity : next),
      })
    }
    return (pre = next)
  }, 0)
  returnValue.push({
    name: dataArray[includeChineseCharacters[len - 1]],
    data: dataArray.slice(includeChineseCharacters[len - 1], Infinity),
  })
  return returnValue
})
</script>

<template>
  <div flex h80vh>
    <div flex="1">
      <textarea
        v-model="input"
        class="textarea"
        border="1 rd"
        cols="30"
        rows="10"
        placeholder="input"
      />
    </div>
    <div flex="1">
      <JsonViewer
        :value="output"
        copyable
        preview-mode
        class="textarea"
        overflow-auto
        border="1 rd"
      />
      <!-- <textarea id="" v-model="output" border cols="30" rows="10" placeholder="output" disabled /> -->
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>

<style>
.textarea {
  position: relative;
  outline: none;
  padding: 10px !important;
  width: 80%;
  height: 100%;
  background: none;
}
 .dark .jv-container.jv-light,
 .dark .jv-array,
 .dark .jv-object,
 .dark .jv-key {
   background: #111 !important;
   color: aliceblue !important;
 }
 .dark .jv-ellipsis {
   background: #111 !important;
   color: aliceblue !important;
 }
 .jv-tooltip .right {
  position:absolute;
 }
</style>
