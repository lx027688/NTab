<template>
    <n-el class="SearchStyle" :style="`transform: scale(${scale});`">
      <n-popselect :options="EngineList" :render-label="renderLabel" @change="SwitchEngine">
        <div class="SearchEngingStyle">
          <img :src="SearchEngine.img">
        </div>
      </n-popselect>
      <input type="text" :placeholder="SearchEngine.placeholder" v-model="SearchContent" @keydown.enter="Search">

      <div class="SearchIconStyle">
        <n-icon size="30" @click="Search">
          <x-search />
        </n-icon>
      </div>
    </n-el>
</template>

<script setup lang='ts'>
import { ref, h } from 'vue'
import { NEl } from 'naive-ui'
import { Search as XSearch } from '@vicons/tabler'
import { NPopselect, NIcon } from 'naive-ui'
import { EngineList } from '../assets/EngineList' //搜索引擎列表
import { mainStore } from '../store/index'
import { storeToRefs } from 'pinia';
const store = mainStore() //引进Pinia仓库
let { XGN_SET } = storeToRefs(store)// 解构出来【设置样式

//搜索引擎的默认设置
let SearchEngine: any = ref(XGN_SET.value.Search_Engine)
//搜索引擎的切换窗口的状态
let SwitchModal_active = ref(false)

const renderLabel = (option: any) => {
  return h('div',[
    h('img', {
      src: `${option.img}`,
      style: {
        width: '20px',
        'margin-right': '10px'
      }
    }),
    h('div', {
      style: {
        float: 'right'
      }
    },`${option.label}`),
  ])
}

//切换搜索引擎
const SwitchEngine = (value: object) => {
  let engine = EngineList.filter(x=>x.value == value)
  if (engine && engine.length > 0) {
    let item = engine[0]
    SearchEngine.value = item
    XGN_SET.value.Search_Engine = item
    SwitchModal_active.value = false
  }
}
//搜索！
let SearchContent = ref('')
const Search = () => {
    window.open(SearchEngine.value.key + SearchContent.value);
    SearchContent.value = "";
}

// 搜索框大小
let scale = ref('1')

</script>

<style lang='scss' scoped>
.SearchStyle {
    width: 90%;
    max-width: 800px;
    min-width: 300px;
    height: 24%;
    min-height: 50px;
    display: flex;
    border-radius: 50px;
    background-color: var(--body-color);
    box-shadow: 2px 2px 16px -6px #333;
    overflow: hidden;
}

input {
    display: block;
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    border: none;
    border-radius: 5px;
    font-size: 20px;
    background-color: var(--body-color);
    color: var(--text-color);
    padding-right: 10px;
}

input:focus {
    outline: none;
}

input:hover {
    border: none;
    box-sizing: border-box;
}

.SearchEngingStyle {
    width: 70px;
    height: 100%;
    background-color: var(--body-color);
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    img {
        height: 50%;
    }
}

.SearchIconStyle {
  width: 70px;
  height: 100%;
  background-color: var(--body-color);
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  img {
    height: 50%;
  }
}
</style>