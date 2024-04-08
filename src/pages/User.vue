<template>
  <van-cell title="头像" is-link @click="toEdit('avatarUrl', '头像', user.avatarUrl)">
    <img style="height: 48px" :src="user.avatarUrl" alt="头像" />
  </van-cell>
  <van-cell title="用户名" is-link :value="user.username" @click="toEdit('username', '用户名', user.username)"/>
  <van-cell title="账号" :value="user.userAccount" />
  <van-cell title="性别" is-link :value="user.gender" @click="toEdit('gender', '性别', user.gender)"/>
  <van-cell title="电话" is-link :value="user.phone" @click="toEdit('phone', '电话', user.phone)"/>
  <van-cell title="邮箱" is-link :value="user.email" @click="toEdit('email', '邮箱', user.email)"/>
  <van-cell title="编号" arrow-direction="down" :value="user.planetCode" />
  <van-cell title="注册时间" arrow-direction="down" :value="user.createTime.toISOString()" />
</template>

<script setup lang="ts">
import {useRouter} from "vue-router";

const user = {
    id: 1,
    username: 'xiongyuqing',
    userAccount: 'rain-sun',
    avatarUrl: 'https://xiongyuqing-img.oss-cn-qingdao.aliyuncs.com/img/202401231713002.jpg',
    gender:'女',
    phone :'123123123',
    email :'rainsun.@xx.com',
    createTime: new Date(),
    planetCode:'1232'
}

const router = useRouter();
const toEdit = (editKey:string, editName:string, currentValue:string)=>{
  router.push({
    path: '/user/edit',
    query:{
      editKey,
      editName,
      currentValue,
    }
  })
}

</script>

<style>

</style>