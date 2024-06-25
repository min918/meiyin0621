<script setup lang="ts">
import { onMounted, ref } from 'vue'
import jsonData from './../data/data1.json';
import optionsData from './../data/data3.json';

const select = ref('')

interface RestaurantItem {
  value: string,
  fun1: string,
  time1: string,
  price1: string,
  req1: string,
  factor1: string,
  sign1: string,
  mark1: string
}

interface Items {
  value1: string,
  label1: string
}

interface data {
  value1: string,
  label1: string,
  data: []
}

const datas = ref<data>()

const options = ref<Items[]>([])

const state1 = ref('')
const state2 = ref('')
const state3 = ref('')
const state4 = ref('')
const state5 = ref('')
const state6 = ref('')
const state7 = ref('')
const state8 = ref('')
const state9 = ref('')
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
  state5.value = item.price1
  state6.value = item.req1
  state7.value = item.factor1
  state8.value = item.sign1
  state9.value = item.mark1
}

const handleClear = (item: RestaurantItem) => {
  isShow.value = false
}

const handleChange = (item: Items) => {
  const results = optionsData.filter(item1 => item1.value1 === item);
  restaurants.value = results[0].data
  state1.value = ''
  isShow.value = false
}

onMounted(() => {
  options.value = optionsData
})

</script>

<template>
  <!-- <div w="full" h="full" p="4" bg="gray-100"> -->

  <h1 color="$ep-color-primary">项目查询</h1>

  <!-- <el-autocomplete
    v-model="state1"
    :fetch-suggestions="querySearch"
    popper-class="my-autocomplete"
    placeholder="Please input"
    @select="handleSelect"
  >
    <template #prepend>
      <el-select v-model="select" placeholder="Select" style="width: 100px">
          <el-option label="肝胆功能检测" value="1" />
          <el-option label="肾功能检测" value="2" />
          <el-option label="电解质检测" value="3" />
        </el-select>
    </template>
<template #default="{ item }">
      <div class="value">{{ item.value }}</div>
      <span class="link">{{ item.link }}</span>
    </template>
</el-autocomplete> -->

  <div class="my-autocomplete">
    <el-autocomplete size="default" v-model="state1" :fetch-suggestions="querySearch" clearable placeholder="输入关键字"
    :popper-append-to-body="false" @select="handleSelect" @clear="handleClear">

      <template #prepend>
        <el-select size="default" v-model="select" style="width: 80px;" placeholder="一级项目" @change="handleChange">
          <el-option v-for="item in options" :key="item.value1" :label="item.label1" :value="item.value1" />
        </el-select>
      </template>
      <!-- <template #default="{ item }">
      <div class="value">{{ item.value }}</div>
      <span class="link">{{ item.link }}</span>
    </template> -->
    </el-autocomplete>
  </div>

  <!-- <el-autocomplete size="large" class="inline-input" v-model="state1" :fetch-suggestions="querySearch" clearable
    placeholder="输入关键字" @select="handleSelect" @clear="handleClear"></el-autocomplete> -->

  <div class="bottom-section" v-if="isShow">
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">检测项目:</el-text>
        <el-text size="large"> {{ state2 }}</el-text></span>
      <el-divider />

    </el-row>
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">检测方法: </el-text>
        <el-text size="large"> {{ state3 }}</el-text></span>
      <el-divider />

    </el-row>

    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">报告时间: </el-text>
        <el-text size="large"> {{ state4 }}</el-text></span>
      <el-divider />

    </el-row>


    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">标准价格（元）: </el-text>
        <el-text size="large"> {{ state5 }}</el-text></span>
      <el-divider />

    </el-row>

    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">标本要求: </el-text>
        <el-text size="large"> {{ state6 }}</el-text></span>
      <el-divider />

    </el-row>
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">标本保存条件: </el-text>
        <el-text size="large"> {{ state7 }}</el-text></span>
      <el-divider />

    </el-row>

    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">临床意义: </el-text>
        <el-text size="large"> {{ state8 }}</el-text></span>
      <el-divider />

    </el-row>

    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">备注: </el-text>
        <el-text size="large"> {{ state9 }}</el-text></span>
      <el-divider />

    </el-row>

  </div>

</template>

<style scoped>
.my-autocomplete {
  display: flex;
  width: 90%;
  padding: 10px;
  margin-left: 10px;
  margin-right: 10px;
}

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
  padding: 1.5rem;
}
</style>