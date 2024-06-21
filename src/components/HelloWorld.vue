<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { ElMessage } from "element-plus";
import jsonData from './../data/project.json';

interface RestaurantItem {
  value: string
}

const state1 = ref('')
const state2 = ref('')

const restaurants = ref<RestaurantItem[]>([])
const querySearch = (queryString: string, cb: any) => {
  const results = queryString
    ? restaurants.value.filter(createFilter(queryString))
    : restaurants.value
  // call callback function to return suggestions
  cb(results)
}
const createFilter = (queryString: string) => {
  return (restaurant: RestaurantItem) => {
    return (
      restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
    )
  }
}
// const loadAll = jsonData;

const loadAll = () => {
  return jsonData;
}

const handleSelect = (item: RestaurantItem) => {
  console.log(item)
  state2.value = item.value
}

onMounted(() => {
  restaurants.value = loadAll()
})

</script>

<template>
  <!-- <div w="full" h="full" p="4" bg="gray-100"> -->

    <h1 color="$ep-color-primary">项目查询</h1>

    <el-autocomplete size="large" v-model="state1" :fetch-suggestions="querySearch" clearable class="inline-input w-80 m-2"
      placeholder="输入关键字" @select="handleSelect" />

    <div class="bottom-section">
      <el-text class="mx-2" type="primary">{{ state2 }}</el-text>
    </div>
  <!-- </div> -->
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
  justify-content: center;
  flex-wrap: wrap;
  gap: 15px;
  padding: 30px;
}
</style>
