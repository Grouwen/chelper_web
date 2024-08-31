<template>
  <div class="container">
    <el-affix position="top">

      <el-row style="height: 10%">
        <el-col :span="24">

            <BaseHeader></BaseHeader>

        </el-col>
      </el-row>

    </el-affix>

    <el-row style="height: 90%;">
      <el-col :span="24">

        <router-view></router-view>

      </el-col>
    </el-row>

  </div>
</template>

<script setup>
import BaseHeader from "@/components/BaseHeader.vue";

</script>

<style scoped>

.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.container {
  display: grid;
  height: calc(100vh - 10px);
  grid-template-rows: auto 1fr auto;
}

</style>
