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

defineProps<{ msg: string }>();

const count = ref(0);
const input = ref("element-plus");

const curDate = ref("");

const toast = () => {
  ElMessage.success(state2.value);
};

const value1 = ref(true);
</script>

<template>
  <h1 color="$ep-color-primary">项目查询</h1>

  <!-- <p>
    See
    <a href="https://element-plus.org" target="_blank">element-plus</a> for more
    information.
  </p> -->

  <!-- <el-divider>
    <el-icon><star-filled /></el-icon>
  </el-divider> -->

  <el-autocomplete size="large" v-model="state1" :fetch-suggestions="querySearch" clearable class="inline-input w-100"
    placeholder="输入关键字" @select="handleSelect" />

  <div class="bottom-section">
    <el-text class="mx-2" type="primary">{{ state2 }}</el-text>
  </div>

  <!-- example components -->
  <!-- <div class="mb-4">
    <el-button size="large" @click="toast">El Message</el-button>
  </div> -->

  <!-- <div class="my-2 text-center flex flex-wrap justify-center items-center">
    <el-button @click="count++">count is: {{ count }}</el-button>
    <el-button type="primary" @click="count++">count is: {{ count }}</el-button>
    <el-button type="success" @click="count++">count is: {{ count }}</el-button>
    <el-button type="warning" @click="count++">count is: {{ count }}</el-button>
    <el-button type="danger" @click="count++">count is: {{ count }}</el-button>
    <el-button type="info" @click="count++">count is: {{ count }}</el-button>
  </div> -->

  <!-- <div>
    <el-switch v-model="value1" />
    <el-switch
      v-model="value1"
      class="m-2"
      style="--ep-switch-on-color: black; --ep-switch-off-color: gray;"
    />
  </div> -->

  <!-- <div class="my-2">
    <el-input class="m-2" v-model="input" style="width: 200px" />
    <el-date-picker
      class="m-2"
      v-model="curDate"
      type="date"
      placeholder="Pick a day"
    ></el-date-picker>
  </div> -->

  <!-- <p>For example, we can custom primary color to 'green'.</p>

  <p>
    Edit
    <code>components/HelloWorld.vue</code> to test components.
  </p>
  <p>
    Edit
    <code>styles/element/var.scss</code> to test scss variables.
  </p>

  <p>
    Full Example:
    <a
      href="https://github.com/element-plus/element-plus-vite-starter"
      target="_blank"
      >element-plus-vite-starter</a
    >
    | On demand Example:
    <a
      href="https://github.com/element-plus/unplugin-element-plus"
      target="_blank"
      >unplugin-element-plus/examples/vite</a
    >
  </p> -->
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
