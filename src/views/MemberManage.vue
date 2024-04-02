<template>
  <div class="member-manage-container">
    <OrgTree @checkOrgId="getOrgId" @checkOrgName="getOrgName"></OrgTree>
    <UserTable
      :checkedOrgId="checkedOrgId"
      :checkOrgName="checkOrgName"
    ></UserTable>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import OrgTree from './../components/OrgTree.vue'
import UserTable from './../components/UserTable.vue'

let checkedOrgId = ref('')
let checkOrgName = ref('')

const getOrgId = (orgId: string) => {
  if (orgId) {
    checkedOrgId.value = orgId
  }
}

const getOrgName = (orgName: string) => {
  if (orgName) {
    checkOrgName.value = orgName
  }
}
</script>

<style scoped lang="scss">
.member-manage-container {
  display: flex;
  .page-container {
    flex: 1;
    height: 100%;
  }
}
</style>
