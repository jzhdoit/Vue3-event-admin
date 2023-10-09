<script setup>
import { artGetChannelsService } from '@/api/article'
import { ref } from 'vue'

defineProps({
  modelValue: {
    type: [Number, String]
  },
  width: {
    type: String
  }
})
const emit = defineEmits(['update:modelValue'])
const channelList = ref([])
const getChannelList = async () => {
  const res = await artGetChannelsService()
  channelList.value = res.data.data
  console.log(channelList)
}
getChannelList()
</script>
<template>
  <!-- label展示给用户看的 value提交给后台的 -->
  <el-select
    :style="{ width }"
    :modelValue="modelValue"
    @update:modelValue="emit('update:modelValue', $event)"
  >
    <el-option
      v-for="channel in channelList"
      :key="channel.id"
      :label="channel.cate_name"
      :value="channel.id"
    ></el-option>
  </el-select>
</template>
