<template>
  <van-cell title="头像" is-link to="/user/edit">
    <img style="height: 48px" :src="user.avatarUrl" alt="头像"/>
  </van-cell>
  <van-cell title="用户名" is-link to="/user/edit" :value="user.username"/>
  <van-cell title="账号" :value="user.userAccount" />
  <van-cell title="性别" is-link to="/user/edit" :value="user.gender" />
  <van-cell title="电话" is-link to="/user/edit" :value="user.phone" />
  <van-cell title="邮箱" is-link to="/user/edit" :value="user.email" />
  <van-cell title="编号" arrow-direction="down" :value="user.planetCode" />
  <van-cell title="注册时间" arrow-direction="down" :value="user.createTime.toISOString()" />
</template>

<script setup lang="ts">
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
</script>

<style>

</style>