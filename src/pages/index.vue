<script setup lang="ts">
import { JsonViewer } from 'vue3-json-viewer'
import { dataType } from '~/composables/dataType'
import 'vue3-json-viewer/dist/index.css'
const input = ref<string>(`AQ - 违规:
irenewobinson.com
thomasborman.com
youvanwinkle.com
laraepop.com
zuopatricia.com
annybabu.com
samsome.com
gulugulo.com
danaedance.com
grindaeay.com
louisekoop.com
xxdiandian.com
quellwoow.com
cecilialool.com
ulricwgooe.com
izefiasuup.com
mashhua.com
dalateresive.com
brianarry.com
kathharr.com
deolivra.com
anniraig.com
specialbrowse.com
itsoftcore.com
brumgame.com
kindneygame.com
senhaiitnews.com
uatehorse.com
suitabfinance.com
singlehopeli.com
vanishcar.com
figuhorse.com
preshorse.com
rcephorse.com
lawaimee.com
crazygames.vip
rrorhorse.com
webaimee.com
hotaimee.com
envehorse.com
mailaimee.com
reaterluck.com
runcooks.com
quuitnews.com
tplkabouts.com
unsafeexit.com
wiseaut.com
photern.com
undershes.com
borsgame.com
ousfinance.com
dipityfiance.com
minisfinance.com
demagame.com
agintigames.com
dgingames.com
sentiter.com
ravigame.com
leleigame.com
charfinance.com
hawergames.com
tallegame.com
cophgames.com
werifinance.com
shinfinance.com
cheefinance.com
tritang.com



AQ - 缺ads.txt:
ejoyzone.com
game3h.com


AQ - 已下线，无法访问:
quipgamer.com
mockhorse.com
@王佳峰
`)
const output = computed(() => {
  if (!input.value)
    return ''
  const dataArray = input.value
    .trim()
    .replace(/\..*\n/g, '\n')
    .split('\n')
    .filter(Boolean)
  const includeChineseCharacters: number[] = []
  let returnValue: any = []
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
  // xxx
  if (len !== 1) {
    returnValue.push({
      name: dataArray[includeChineseCharacters[len - 1]],
      data: dataArray.slice(includeChineseCharacters[len - 1] + 1, Infinity),
    })
  }
  const type = dataType.value.find(item => item.show)
  if (type && type.label !== 'json')
    returnValue = returnValue.map(el => type.type.some(tl => el.name.includes(tl)) && el.data.join(',')).filter(Boolean).join(',')
  return returnValue
})
</script>

<template>
  <button-type />
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
