<template>
  <div class="user-table-container">
    <div class="table-nav">
      <a-breadcrumb>
        <a-breadcrumb-item href="">
          <user-outlined />
          <span>厦门嗨行旅游</span>
        </a-breadcrumb-item>
        <a-breadcrumb-item v-if="props.checkOrgName">{{
          props.checkOrgName
        }}</a-breadcrumb-item>
      </a-breadcrumb>
    </div>

    <div class="user-table">
      <a-table :columns="columns" :data-source="data" bordered>
        <template #bodyCell="{ column, text }">
          <template v-if="column.dataIndex === 'name'">
            <a>{{ text }}</a>
          </template>
        </template>
        <template #title>
          <a-input
            v-model:value="searchValue"
            :bordered="false"
            placeholder="按Enter搜索"
            @pressEnter="handleFilter"
          >
            <template #prefix><SearchOutlined /></template>
          </a-input>
        </template>
        <template #footer>Footer</template>
      </a-table>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { UserOutlined, SearchOutlined } from '@ant-design/icons-vue'

import { ref, watch } from 'vue'
import userApi from '../api/user'
import { throttle } from '../utils/index'

const props = defineProps<{ checkedOrgId: string; checkOrgName: string }>()

const searchValue = ref<string>('')

const columns = [
  {
    title: '姓名',
    dataIndex: 'name',
  },
  {
    title: '用户名',
    className: 'userName',
  },
]

let data = ref([])
let rawData = ref([])

watch(
  () => props.checkedOrgId,
  (newValue) => {
    if (newValue) {
      searchValue.value = ''
      userApi.query({}).then((users) => {
        if (users.length) {
          let newData = []
          users.forEach((item, index) => {
            newData.push({
              key: index,
              name: item.name,
              userName: item.name,
              parentId: newValue,
              id: item.id,
            })
          })
          data.value = [...newData]
          rawData.value = JSON.parse(JSON.stringify(newData))
        }
      })
    }
  },
  { immediate: true }
)

const handleFilter = throttle((e) => {
  let filerData = []
  if (e.target.value && data?.value?.length) {
    data.value = rawData.value
    filerData = data.value.filter((item) => item.name.includes(e.target.value))
    data.value = filerData
  } else {
    data.value = rawData.value
  }
}, 1000)
</script>

<style scoped lang="scss">
.user-table-container {
  width: calc(100% - 260px);
  border-right: 1px solid rgba(5, 5, 5, 0.06);
  border-bottom: 1px solid rgba(5, 5, 5, 0.06);
}
.table-nav {
  width: 100%;
  height: 40px;
  padding: 0 12px;
  border-bottom: 1px solid rgba(5, 5, 5, 0.06);
  .ant-breadcrumb {
    height: 100%;
    display: flex;
    align-items: center;
  }
}

th.column-money,
td.column-money {
  text-align: right !important;
}
</style>
