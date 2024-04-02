<template>
  <div class="org-tree-container">
    <div class="org-header">
      <span>部门</span>
      <div>
        <PlusOutlined />
      </div>
    </div>
    <a-tree
      v-model:expandedKeys="expandedKeys"
      v-model:selectedKeys="selectedKeys"
      :load-data="onLoadData"
      :tree-data="treeData"
      @select="loadUsersByOrgId"
    />
  </div>
</template>
<script lang="ts" setup>
import { ref, defineEmits } from 'vue'
import { PlusOutlined } from '@ant-design/icons-vue'
import type { TreeProps } from 'ant-design-vue'
import { debounce } from '../utils/index'

import orgApi from '../api/org'

const expandedKeys = ref<string[]>([])
const selectedKeys = ref<string[]>([])
const treeData = ref<TreeProps['treeData']>([
  {
    title: '厦门嗨行旅游',
    key: '0',
  },
  { title: '未分配部门', key: '2', isLeaf: true },
])
const onLoadData: TreeProps['loadData'] = (treeNode: any) => {
  return new Promise<void>((resolve) => {
    if (treeNode.dataRef.children) {
      resolve()
      return
    }

    setTimeout(() => {
      orgApi.query('1').then((users) => {
        const newChildren = []
        if (users.length) {
          users.forEach((item: Object) => {
            newChildren.push({
              title: item['name'],
              key: item['id'],
              isLeaf: true,
            })
          })
          treeNode.dataRef.children = newChildren
          treeData.value = [...treeData.value]
          resolve()
        }
      })
    }, 1000)
  })
}

const emit = defineEmits(['checkOrgId', 'checkOrgName'])

const loadUsersByOrgId = debounce((selectedKeys, e) => {
  if (e?.node?.parent?.key === '0') {
    emit('checkOrgId', selectedKeys)
    emit('checkOrgName', e?.node?.title)
  }
}, 500)
</script>

<style scoped lang="scss">
.org-tree-container {
  width: 260px;
  border: 1px solid rgba(5, 5, 5, 0.06);
  border-top: none;
}
.org-header {
  width: 100%;
  height: 40px;
  padding: 0 12px;
  line-height: 40px;
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid rgba(5, 5, 5, 0.06);
}
</style>
