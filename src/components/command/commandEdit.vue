<template>
  <div>
<!--    <footer>-->
<!--      <button class="button custom-font" @click="openSettings">设置</button>-->
<!--      <input ref="inputRef" class="input-box custom-font" placeholder="请输入内容" v-model="input"-->
<!--             @input="onTextChanged">-->
<!--      <button class="button custom-font" @click="copy">复制</button>-->
<!--    </footer>-->
    <el-row>
      <el-col :span="1.5">

        <el-button type="primary" size="large" @click="openSettings">设置</el-button>

      </el-col>
      <el-col :span="21">

        <el-input v-model="input"
                  placeholder="请输入内容"
                  style="height: 100%"
                  @input="onTextChanged"
                  ref="inputRef"/>

      </el-col>
      <el-col :span="1.5">

        <el-button type="primary" size="large" @click="copy">复制</el-button>

      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import {ref} from "vue";

const input = ref("");
const inputRef = ref()

const emit = defineEmits(['onTextChanged'])
const openSettings = function() {
  window.alert("暂时还没有设置")
};

const getInputRef = function (){
  return inputRef.value
}

const getInputData = function (){
  return input.value
}

const upDateInput = function (newData){
  input.value = newData
}

const onTextChanged = function (){
  emit("onTextChanged")
}

const copy = function (){
  if (input.value){
    inputRef.value.select(); // 选中文本
    document.execCommand('copy');
    ElMessage({
      message: '已复制',
      grouping: true,
      type: 'success',
    })
  }else {
    ElMessage({
      message: '复制失败，无内容',
      grouping: true,
      type: 'warning',
    })
  }

  input.value = ""

  onTextChanged()
}


defineExpose({
  input,
  getInputRef,
  upDateInput,
  getInputData
})
</script>

<style scoped>

.custom-font {
  font-size: 15px;
  font-family: Inter, 'Helvetica Neue', Helvetica, 'PingFang SC',
  'Hiragino Sans GB', 'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
}

.input-box {
  margin: 0 5px 0 5px;
  width: calc(100vw - 140px);
  height: auto;
  color: black;
  text-align: left;
  background-color: #f5f7fa;
  padding: 10px;
  border: 0;
  border-radius: 5px;
}

.input-box:focus {
  outline: 2px solid dodgerblue;
}
</style>
