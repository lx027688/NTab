<template>
    <n-space vertical :size="20" style="margin:10px 0">
        <n-card size="large" hoverable>
            <n-space class="flex">
                <n-avatar :size="60" round src="./XGN.png" style="margin-right:20px" />
                <n-space vertical>
                    <n-gradient-text type="info" class="h1">
                        注册/登录
                    </n-gradient-text>
                    <div>
                        即刻享受更多特权
                    </div>
                </n-space>
            </n-space>
        </n-card>

        <n-card title="备份与恢复" hoverable>
          <n-grid x-gap="50" y-gap="50" :cols="6">
            <n-gi :key="1">
              <n-button type="primary" @click="downloadConfig"> 下载配置 </n-button>
            </n-gi>
            <n-gi :key="1">
              <n-upload ref="refUpload" action="http://127.0.0.1:8080" :max="1" :multiple="false" :default-upload="false" :show-file-list="false"
                        @before-upload="beforeUpload"  @change="uploadConfig">
                <n-button type="info"> 上传配置 </n-button>
              </n-upload>
<!--              <input type="file" ref="fileInput" @change="uploadConfig" style="font-size: 400;height: 34px;">-->
<!--              <n-upload @change="uploadConfig" ref="upload"
                        action="https://www.mocky.io/v2/5e4bafc63100007100d8b70f"
                        :default-upload="false">
                  <n-button type="info"> 上传配置 </n-button>
              </n-upload>-->

            </n-gi>
          </n-grid>

        </n-card>
        <n-card title="关于" hoverable>
            <n-space vertical :size="20">
                <n-tag> v0.0.1 </n-tag>
                <n-space :size="20" class="aboutMe">
                    <a href="https://github.com/XXGGG" target="_blank">
                        <n-avatar :size="60" src="./book_mark_img/github.png" />
                    </a>
                    <a href="https://space.bilibili.com/5276030" target="_blank">
                        <n-avatar :size="60" src="./book_mark_img/bilibili.png" />
                    </a>
                    <a href="https://www.weibo.com/u/2472496944" target="_blank">
                        <n-avatar :size="60" src="./book_mark_img/weibo.png" />
                    </a>
                    <a href="https://xxggg.github.io/Project/XGNavigation.html" target="_blank">
                        <n-avatar :size="60" src="./XGN.png" />
                    </a>
                </n-space>
            </n-space>
        </n-card>
    </n-space>
</template>

<script setup lang='ts'>
import {NCard, NAvatar, NSpace, NGradientText, NTag, NGrid, NGi, NButton, NUpload } from 'naive-ui'
import { createDiscreteApi } from 'naive-ui'
import { mainStore } from '../../store/index'
import {storeToRefs} from "pinia";
const store = mainStore() //引进Pinia仓库
let { XGN_SET } = storeToRefs(store)// 解构出来【设置样式】，【我的书签】

const downloadConfig = () => {
  let config = XGN_SET.value;

  const json = JSON.stringify(config, null, 2);
  const blob = new Blob([json], { type: 'application/json' });
  const url = URL.createObjectURL(blob);

  const a = document.createElement('a');
  a.href = url;
  a.download = 'config.json';
  a.click();
  URL.revokeObjectURL(url);
}

function beforeUpload (file, fileList) {
  const {message} = createDiscreteApi(["message"])
  if (file.file.file?.size > 1024*1024*10) { // 文件大小超过10M
    message.warning('上传文件大小不能超过10M')
    return false
  }
  if (file.file?.type !== 'application/json') { // 文件不是json
    message.warning('上传文件类型必须为json')
    return false
  }
  message.success('配置上传成功！')
  return true
}

const uploadConfig =async ({file, fileList, event}) => {
  const content = await readFileContent(file.file);
  XGN_SET.value = JSON.parse(content)
}

function readFileContent(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();

    reader.onload = () => {
      const content = reader.result;
      resolve(content);
    };

    reader.onerror = (error) => {
      reject(error);
    };

    reader.readAsText(file);
  });
}
</script>

<style lang='scss' scoped>
.h1 {
    font-size: 18px;
    font-weight: bold;
}

.aboutMe {
    a { 
        display: block;
        width: 60px;
        height: 60px;
        transition:opacity .2s;
        border-radius: 14px;
        overflow: hidden;
        &:hover {
            opacity: 0.6;
        }
    }
}
</style>