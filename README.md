[TOC]

# 页面效果

## 搜索：

<img src="https://xiongyuqing-img.oss-cn-qingdao.aliyuncs.com/img/202404090032326.png" alt="image-20240409003207160" style="zoom:50%;" />

## 个人：

<img src="https://xiongyuqing-img.oss-cn-qingdao.aliyuncs.com/img/202404090031014.png" alt="image-20240409003122841" style="zoom:50%;" />

# 前端初始化

## Vue & Vite 脚手架初始化项目

[开始 | Vite 官方中文文档 (vitejs.cn)](https://vitejs.cn/vite3-cn/guide/#scaffolding-your-first-vite-project) （类似 vue 的前端框架）

使用 vite 前端框架初始化项目：

```sh
npm create vite@latest
```

## 引入 Vant 前端组件库

[快速上手 - Vant 3 (gitee.io)](https://vant-contrib.gitee.io/vant/v3/#/zh-CN/quickstart) (移动端组件库)

1. 安装 Vant

```sh
npm i vant
```

2. vant3 按需引入组件样式：

```sh
npm i unplugin-vue-components -D
```

3. 基于 `vite` 的项目，在 `vite.config.ts` 文件中配置插件：

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import Components from 'unplugin-vue-components/vite';
import { VantResolver } from 'unplugin-vue-components/resolvers';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    Components({
      resolvers: [VantResolver()],
    }),
  ],
})
```

4. 引入组件样式，并使用

全局注册组件（这里使用 Button 组件）

```ts
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import { Button } from 'vant';

const app = createApp(App);
app.use(Button);    
app.mount('#app')
```

App.vue 中引入组件样式：

```html
<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <HelloWorld msg="Vite + Vue" />
  <van-button type="primary">主要按钮</van-button>
  <van-button type="success">成功按钮</van-button>
  <van-button type="default">默认按钮</van-button>
  <van-button type="warning">警告按钮</van-button>
  <van-button type="danger">危险按钮</van-button>
</template>
```

5. 效果：

<img src="https://xiongyuqing-img.oss-cn-qingdao.aliyuncs.com/img/202404021653452.png" alt="image-20240402165334875" style="zoom: 25%;" />

## 项目结构

<img src="https://xiongyuqing-img.oss-cn-qingdao.aliyuncs.com/img/202404031558499.png" alt="image-20240403155832444" style="zoom:50%;" />

# 页面设计

## 公共组件

### 上下导航栏

1. `main.ts` 中引入要使用的组件库样式：

```ts
import { createApp } from 'vue'
import App from './App.vue'
import {Icon, NavBar, Tabbar, TabbarItem} from 'vant';

const app = createApp(App);
app.use(NavBar);
app.use(Icon);
app.use(Tabbar);
app.use(TabbarItem);
app.mount('#app')
```

2. `BasicLayout.vue` 中使用引入的组件

```vue
<template>
  <van-nav-bar
      title="标题"
      right-text="按钮"
      left-arrow
      @click-left="onClickLeft"
      @click-right="onClickRight"
  >
    <template #right>
      <van-icon name="search" size="18" />
    </template>
  </van-nav-bar>

  <div id="content">
    <template v-if="active === 'index'">
      <Index></Index>
    </template>
    <template v-if="active === 'team'">
      <Team></Team>
    </template>
  </div>

  <van-tabbar v-model="active" @change="onChange">
    <van-tabbar-item icon="home-o" name="index">主页</van-tabbar-item>
    <van-tabbar-item icon="search" name="team">队伍</van-tabbar-item>
    <van-tabbar-item icon="friends-o" name="user">个人</van-tabbar-item>
  </van-tabbar>

</template>

<script setup>
import { ref } from 'vue';
import {Toast} from "vant";
import Index from "../pages/Index.vue";
import Team from "../pages/Team.vue";

const onClickLeft = () => alert("left");
const onClickRight = () => alert("right");

const active = ref("index");
const onChange = (index) => Toast(`标签 ${index}`);

</script>

<style scoped>

</style>
```

3. App.vue 中使用 BasicLayout:

```vue
<script setup lang="ts">
import BasicLayout from "./layouts/BasicLayout.vue";
</script>

<template>
  <BasicLayout></BasicLayout>
</template>

<style>

</style>
```

效果：

<img src="https://xiongyuqing-img.oss-cn-qingdao.aliyuncs.com/img/202404031601074.png" alt="image-20240403160103981" style="zoom:50%;" />

页面切换使用 active，下一步使用路由来选择展示不同的页面

### 路由组件

使用 vue-router，根据路径展示页面

```ts
import Index from "../pages/Index.vue";
import Team from "../pages/Team.vue";
import User from "../pages/User.vue";
import Search from "../pages/Search.vue";

//  定义一些路由
const routes = [
    { path: '/', component: Index },
    { path: '/team', component: Team },
    { path: '/user', component: User },
    { path: '/search', component: Search },
]

export default routes;
```

BasicLayout 中的更改，添加 `route` 属性，子标签添加 `to` 属性定义点击跳转的页面：

```vue
  <router-view/>
  <van-tabbar route @change="onChange">
    <van-tabbar-item to="/" icon="home-o" name="index">主页</van-tabbar-item>
    <van-tabbar-item to="/team" icon="search" name="team">队伍</van-tabbar-item>
    <van-tabbar-item to="/user" icon="friends-o" name="user">个人</van-tabbar-item>
  </van-tabbar>
```

# 页面实现

## 搜索页面

标签的数据量很小，所以用前端实现标签搜索：

```vue
<template>
  <form action="/">
    <van-search
        v-model="searchText"
        show-action
        placeholder="请输入搜索的标签"
        @search="onSearch"
        @cancel="onCancel"
    />
  </form>

  <van-divider content-position="center">已选标签</van-divider>
  <div v-if="activeIds.length === 0">请选择标签</div>

  <van-row gutter="16">
    <van-col v-for="tag in activeIds">
      <van-tag closeable size="medium" type="primary" @close="doClose(tag)">
        {{tag}}
      </van-tag>
    </van-col>
  </van-row>

  <van-divider content-position="center">选择标签</van-divider>

  <van-tree-select
      v-model:active-id="activeIds"
      v-model:main-active-index="activeIndex"
      :items="tagList"
  />

</template>

<script setup lang="ts">
import { ref } from 'vue';


const searchText = ref('');

const originTagList = [
  {
    text: '性别',
    children: [
      { text: '男', id: '男' },
      { text: '女', id: '女'},
    ],
  },
  {
    text: '年级',
    children: [
      { text: '大一', id: '大一' },
      { text: '大二', id: '大二' },
      { text: '大三', id: '大三' },
      { text: '大四', id: '大四' },
    ],
  },
  {
    text: '桌游',
    children: [
      { text: '美式', id: '美式' },
    ],
  },
  {
    text: '等级',
    children: [
      { text: '初级', id: '初级' },
    ],
  },
];

let tagList = ref(originTagList);

const onSearch = (val) => {
  tagList.value = originTagList.map(parentTag => {
    const tempChildren = [...parentTag.children];
    const tempParentTag = {...parentTag};
    tempParentTag.children = tempChildren
         .filter(item => item.text.includes(searchText.value))
    return tempParentTag;
  });
}
const onCancel = () => {
  searchText.value = '';
  tagList.value = originTagList;
}

// 已选中的标签
const activeIds = ref([]);
const activeIndex = ref(0);



const doClose = (tag) => {
  activeIds.value = activeIds.value.filter(item => {
    return item != tag;
  })
}

</script>

<style>

</style>
```

## 用户中心

定义用户类型和属性:

```ts
/**
 * 用户类别
 */
export type CurrentUser = {
    id: number;
    username: string;
    userAccount:string;
    avatarUrl?:string;
    gender?:number;
    phone :string;
    email :string;
    userStatus :number;
    createTime:Date;
    userRole :number;
    planetCode?: string;
    tags: string[];
};
```

使用 cell 展示属性：

```vue
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
```

# Bug Record

1. vue3+vite中 不能找到模块 Cannot find module ‘./layouts/BasicLayout.vue‘ or its corresponding type declarations

[vue3+vite中 使用@不能找到模块 Cannot find module ‘@/utils/request‘ or its corresponding type declarations_cannot find module '@/utils/request-CSDN博客](https://blog.csdn.net/weixin_44168929/article/details/130555041)

```ts
declare module "*.vue" {
    import type { DefineComponent } from "vue"
    const vueComponent: DefineComponent<{}, {}, any>
    export default vueComponent
}	
```

2. NavBar 显示异常

在 main.ts 中删掉原有的 style.css 样式文件

```ts
import './style.css'
```

