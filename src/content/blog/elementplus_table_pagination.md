---
title: elememtplus_table_pagination
author: liubl
pubDatetime: 2024-11-19T12:01:14.525Z
featured: false
draft: false
tags:
  - 前端
description: elememtplus_table_pagination
ogImage: ""
---

# 简单写了一个分页组件

## 定义PaginationTable.vue

代码测试
```js
<template>
 <!-- 外部可以自定义列 -->
  <slot></slot>
  <el-pagination @change="props.handleChangePage" layout="prev, pager, next" :page-size=" props.data.size?? 10" :total="props.data.total?? 0" />
</template>

<script setup>


import { ref, watchEffect } from 'vue';
const props = defineProps({
  data: Object, // 分页参数
  handleChangePage: Function, // 页码点击处理函数
})

console.log({...props.data})
</script>

<style>

</style>


```

## 使用
```js

<template>
  <PageNationTable :handleChangePage="handleChangePage" :data="pageData">
     <!-- normal element table here -->
    <el-table :data="tableData" style="width: 100%;" height="100%">
    <el-table-column prop="id" label="id" width="120" />
    <el-table-column prop="roleName" label="角色名" width="120" />
    <el-table-column prop="roleDescription" label="描述" width="120" />
    <el-table-column prop="createTime" label="创建时间" width="200" />
    <el-table-column prop="updateTime" label="更新时间" width="200" />
    </el-table>
  </PageNationTable>
</template>

<script setup>
import PageNationTable from '../components/PageNationTable.vue';
import { reactive, ref } from 'vue'
import http from '../../http'
// fetch data -> get data and page info -> ref([])
//                      ->  show in table 
//                      -> pass to this compoment to show pagination
const tableData = ref([
])

// 定义分页参数
const pageData = reactive({
  page: 1,
  size: 2,
  total: 0
});
// 页码点击事件
function handleChangePage(e) {
  pageData.page = e
  fetchRoles()
}
// 页面数据查询接口
fetchRoles()

async function fetchRoles() {
  const res = await http.get('/api/role/list?' +  new URLSearchParams({pageNum: pageData.page, pageSize: pageData.size}))
  const { list, page, total } = res.data;
  pageData.page = page
  pageData.total = total
  tableData.value = list
}

</script>

<style>

</style>

```