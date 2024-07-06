<template>
 <AppHeader />
  <!-- 频道模块 -->
  <van-tabs>
    <van-tab v-for="item in chnnelList" :key="item.id" :title="item.name" />
  </van-tabs>
  <!-- 视频列表 -->
  <van-list
  v-model:loading="loading"
  :finished="finished"
  finished-text="没有更多了"
  @load="onLoad"
>
  <div class="video-list">
    <AppVideo v-for="item in list" :key="item.bvid" :item="item"></AppVideo>
  </div>
  </van-list>

</template>

<script setup lang="ts">
import type {RootObject} from '@/types/video'
const { data:chnnelList} = await useFetch('/api/chnnel')
const { data:video} = await useFetch('/api/video')
const list = ref<RootObject[]>([]);
const loading = ref(false)
const finished = ref(false)
const page = ref(1)
const size = ref(20)
const onLoad = () => {
  loading.value = true

  const data = video.value?.slice((page.value - 1) * size.value,size.value * page.value) as RootObject[]
  list.value.push(...data)
  loading.value = false;
  page.value++
  if(list.value?.length === video.value?.length){
    finished.value = true 
  }
}
onLoad()
</script>
<style lang="scss">


// 视频列表
.video-list {
  display: flex;
  flex-wrap: wrap;
  padding: 10px 5px;
}

</style>