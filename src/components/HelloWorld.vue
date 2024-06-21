<script setup lang="ts">
import { onMounted, ref } from 'vue'
import jsonData from './../data/data1.json';

interface RestaurantItem {
  value: string,
  fun1: string,
  time1: string,
  req1: string,
  factor1: string,
  sign1: string
}

const state1 = ref('')
const state2 = ref('')
const state3 = ref('')
const state4 = ref('')
const state5 = ref('')
const state6 = ref('')
const state7 = ref('')
const isShow = ref(false)

const restaurants = ref<RestaurantItem[]>([])
const querySearch = (queryString: string, cb: any) => {
  const results = queryString
    ? restaurants.value.filter(createFilter(queryString))
    : restaurants.value
  cb(results)
}
const createFilter = (queryString: string) => {
  return (restaurant: RestaurantItem) => {
    return (
      restaurant.value.toLowerCase().match((queryString.toLowerCase()))
    )
  }
}

const loadAll = () => {
  return jsonData;
}

const handleSelect = (item: RestaurantItem) => {
  console.log(item)
  isShow.value = true
  state2.value = item.value
  state3.value = item.fun1
  state4.value = item.time1
  state5.value = item.req1
  state6.value = item.factor1
  state7.value = item.sign1
}
const handleClear = (item: RestaurantItem) => {
  console.log(item)
  isShow.value = false
}


onMounted(() => {
  restaurants.value = loadAll()
})

</script>

<template>
  <!-- <div w="full" h="full" p="4" bg="gray-100"> -->

  <h1 color="$ep-color-primary">项目查询</h1>

  <el-autocomplete size="large" v-model="state1" :fetch-suggestions="querySearch" clearable
    class="inline-input w-80 m-2" placeholder="输入关键字" @select="handleSelect" @clear="handleClear" />

  <div class="bottom-section" v-if="isShow">
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">检测项目:</el-text>
        <el-text size="large"> {{ state2 }}</el-text></span>

    </el-row>
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">检测方法: </el-text>
        <el-text size="large"> {{ state3 }}</el-text></span>

    </el-row>

    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">报告时间: </el-text>
        <el-text size="large"> {{ state4 }}</el-text></span>

    </el-row>
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">标本要求: </el-text>
        <el-text size="large"> {{ state5 }}</el-text></span>

    </el-row>
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">标本保存条件: </el-text>
        <el-text size="large"> {{ state6 }}</el-text></span>

    </el-row>

    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">临床意义: </el-text>
        <el-text size="large"> {{ state7 }}</el-text></span>

    </el-row>

  </div>

</template>

<style>
.ep-button {
  margin: 4px;
}

.ep-button+.ep-button {
  margin-left: 0;
  margin: 4px;
}

.bottom-section {
  display: flex;
  margin-top: 30px;
  justify-content: flex-start;
  flex-wrap: wrap;
  flex-direction: column;
  gap: 15px;
  padding: 20px;
}
</style>