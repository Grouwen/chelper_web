<template>
  <div>

    <el-row>
      <el-col :span="24">

        <commandList
            ref="cmdlstRef"
            @onSuggestionClick="onSuggestionClick"
            v-if="haveInit"></commandList>

      </el-col>
    </el-row>

    <el-row style="height: 10%">
      <el-col :span="24">

        <el-affix position="bottom">
          <commandEdit
              ref="cmdEdtRef"
              @onTextChanged="onTextChanged"
              v-if="haveInit"></commandEdit>
        </el-affix>

      </el-col>
    </el-row>

  </div>
</template>

<script setup>
import {reactive,ref} from "vue";
import commandList from '@/components/command/commandList.vue'
import commandEdit from '@/components/command/commandEdit.vue'
import getResourceapi from "@/api/GetResourceApi.js";
import {CHelperCore, wasmInitFuture} from "@/libCHelperWeb.js";
import useCoreStore from "@/pinia/stores/coreStores.js";

const suggestions = ref([]);
const realSuggestionSize = ref(0);
let core = reactive({});
const coreStore = reactive(useCoreStore())
const haveInit = ref(false);

const cmdlstRef = ref();
const cmdEdtRef = ref();

const loadWasmModule = async function (){
  getResourceapi.getResource().then(async (res)=>{
    await wasmInitFuture
    coreStore.core = new CHelperCore(new Uint8Array(res.data))
    core = coreStore.core
    // init
    coreStore.structure = "欢迎使用CHelper"
    coreStore.description = "作者：Yancey"
    coreStore.errorReason = ""
    haveInit.value = true
  })
}

const onTextChanged = function () {
  core.onTextChanged(cmdEdtRef.value.getInputData(), cmdEdtRef.value.getInputData().length);
  if (cmdEdtRef.value.getInputData().length === 0) {
    coreStore.structure = "欢迎使用CHelper"
    coreStore.description = "作者：Yancey"
    coreStore.errorReason = ""
  } else {
    coreStore.structure = core.getStructure()
    coreStore.description = core.getDescription()
    coreStore.errorReason = core.getErrorReason()
  }
  realSuggestionSize.value = core.getSuggestionSize()
  console.log(cmdlstRef.value.getListRef().clientHeight)
  if (cmdEdtRef.value.getInputData()==""){
    cmdlstRef.value.loadMore()
  }else {
    loadMore(Math.floor(cmdlstRef.value.getListRef().clientHeight / 25))
  }

}

const init = ()=>{
  loadWasmModule()
}
init()

const onSuggestionClick = function (which) {
  cmdEdtRef.value.getInputRef().focus()
  core.onSuggestionClick(which)
  cmdEdtRef.value.upDateInput(core.getStringAfterSuggestionClick())
  cmdEdtRef.value.getInputRef().selectionStart = cmdEdtRef.value.getInputRef().selectionEnd = core.getSelectionAfterSuggestionClick()
  onTextChanged()
  window.scrollTo(0,0)
}

const loadMore = function (count) {
  suggestions.value = []
  const start = suggestions.value.length
  const end = Math.min(start + count, realSuggestionSize.value)
  cmdlstRef.value.updateCommandLst([])
  for (let i = start; i < end; i++) {
    suggestions.value.push({
      title: core.getSuggestionTitle(i),
      description: core.getSuggestionDescription(i),
    })
  }
  cmdlstRef.value.updateCommandLst(suggestions.value)
}

</script>

<style scoped>

</style>
