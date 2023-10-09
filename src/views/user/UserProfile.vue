<script setup>
import PageContainer from '@/components/PageContainer.vue'
import { useUserStore } from '@/stores'
import { userUpdateInfoService } from '@/api/user.js'
import { ref } from 'vue'

const {
  user: { username, nickname, email, id },
  getUser
} = useUserStore()
const userInfo = ref({ username, nickname, email, id })
const rules = {
  nickname: [
    { required: true, message: '请输入用户昵称', trigger: 'blur' },
    {
      pattern: /^\S{2,10}$/,
      message: '昵称必须是2-10位的非空字符串',
      trigger: 'blur'
    }
  ],
  email: [
    { required: true, message: '请输入用户邮箱', trigger: 'blur' },
    { type: 'email', message: '邮箱格式不正确', trigger: 'blur' }
  ]
}

const formRef = ref()
const onSumbit = async () => {
  const valid = await formRef.value.validate()
  if (valid) {
    //提交后台修改
    await userUpdateInfoService(userInfo.value)
    //通知 user 模块进行数据的更新
    await getUser()
    ElMessage.success('修改成功')
  }
}
</script>
<template>
  <page-container title="基本资料">
    <!-- 表单部分 -->
    <el-row>
      <el-col :span="12">
        <el-form
          :model="userInfo"
          :rules="rules"
          ref="formRef"
          label-width="100px"
          size="large"
        >
          <el-form-item label="登录名称" prop="">
            <input type="text" v-model="userInfo.username" disabled />
          </el-form-item>
          <el-form-item label="用户昵称" prop="nickname">
            <input type="text" v-model="userInfo.nickname" />
          </el-form-item>
          <el-form-item label="用户邮箱" prop="email">
            <input type="text" v-model="userInfo.email" />
          </el-form-item>
          <el-form-item label="" prop="">
            <el-button @click="onSumbit" type="primary">提交修改</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </page-container>
</template>
