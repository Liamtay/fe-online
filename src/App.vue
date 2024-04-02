<template>
  <div class="app-container">
    <a-tabs v-model:activeKey="activeKey">
      <a-tab-pane key="1" tab="成员管理">
        <MemberManage></MemberManage>
      </a-tab-pane>
      <a-tab-pane key="2" tab="团队管理" force-render>团队管理</a-tab-pane>
      <a-tab-pane key="3" tab="职位维护">职位维护</a-tab-pane>
    </a-tabs>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import MemberManage from './views/MemberManage.vue'
const activeKey = ref('1')
</script>

<style scoped lang="scss">
.app-container {
  width: 100%;
  height: 100%;
  display: flex;
  .ant-tabs {
    width: 100%;
  }
  .page-container {
    flex: 1;
    height: 100%;
  }
  :deep .ant-tabs-nav {
    margin: 0 !important;
  }
}
</style>
