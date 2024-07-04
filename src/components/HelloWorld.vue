<script setup lang="ts">
import { onMounted, ref } from 'vue'
import jsonData from './../data/data_0703.json';
import optionsData from './../data/options_0703.json';
import { ElMessage } from 'element-plus'

const select = ref('')

interface RestaurantItem {
  key: string,
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

const options = ref<Items[]>([])

const sign1 = ref('')

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

const data_0703 = ref<RestaurantItem[]>([])

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

const handleChange = (item: Items) => {
  item.value1
  const results = jsonData.filter(item1 => item1.key === item);
  restaurants.value = results
  state1.value = ''
  isShow.value = false
}

onMounted(() => {
  options.value = optionsData
  restaurants.value = jsonData
})


const doSearch = (cb: any) => {
  const param0 = select
  const param1 = state1
  const param2 = sign1

  data_0703.value = jsonData;
  const results = data_0703;
  let isEvery = results
  if (null != param0.value) {
    isEvery.value = isEvery.value.filter(item1 => item1.key.toLowerCase().match(param0.value));
  }

  if (null != param1.value) {
    // console.log(param1.value)
    // isEvery.value.forEach(element => {
    //   if(element.value.indexOf(param1.value.substring(0,5)) >-1){
    //     console.log(element.value)
    //     console.log(param1.value)
    //     console.log("===>"+element.value.includes(param1.value))
    //   }
    // });
    isEvery.value = isEvery.value.filter(item1 => item1.value.includes(param1.value));
  }

  if (null != param2.value) {
    isEvery.value = isEvery.value.filter(item1 => item1.sign1.toLowerCase().match(param2.value));
  }

  // console.log(isEvery)
  if (null != isEvery.value[0]) {
    const tmp = isEvery.value[0]
    isShow.value = true
    state2.value = tmp.value
    state3.value = tmp.fun1
    state4.value = tmp.time1
    state5.value = tmp.price1
    state6.value = tmp.req1
    state7.value = tmp.factor1
    state8.value = tmp.sign1
    state9.value = tmp.mark1
  } else {
    isShow.value = false
    ElMessage({
      showClose: true,
      message: '查无数据！',
      center: true,
    })
  }

}

</script>

<template>
  <!-- <div w="full" h="full" p="4" bg="gray-100"> -->

  <h1 color="$ep-color-primary">项目册</h1>
  <h4 color="$ep-color-success">专业项目查询助手</h4>

  <div class="m-4">
    <p style="text-align: left;font-weight: bold;">项目分类：</p>
    <el-select round size="default" v-model="select" placeholder="一级项目" @change="handleChange">
      <el-option v-for="item in options" :key="item.value1" :label="item.label1" :value="item.value1" />
    </el-select>
  </div>
  <div class="m-4">
    <p style="text-align: left;font-weight: bold;">项目名称：</p>
    <el-autocomplete size="default" v-model="state1" :fetch-suggestions="querySearch" clearable placeholder="输入关键字"
      :popper-append-to-body="false" @select="handleSelect" @clear="handleClear" />
  </div>
  <div class="m-4">
    <p style="text-align: left;font-weight: bold;">临床意义：</p>
    <el-input v-model="sign1" placeholder="输入关键字，然后点击查询" />
  </div>
  <div class="m-4">
    <!-- <el-button size="large" round>Search</el-button> -->
    <el-button type="success" round @click="doSearch">查询</el-button>
  </div>

  <div class="bottom-section" v-if="isShow">
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">检测项目:</el-text>
        <el-text size="default"> {{ state2 }}</el-text></span>
      <el-divider />

    </el-row>
    <el-row>
      <span><el-text class="mx-1" type="warning" size="large">检测方法: </el-text>
        <el-text size="default"> {{ state3 }}</el-text></span>
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