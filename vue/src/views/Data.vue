<template>
  <div>
    <div class="card" style="margin-bottom: 5px ">
      <el-input style="width: 240px; margin-right: 10px" v-model="data.name" placeholder="请输入名称查询" prefix-icon="Search"></el-input>
      <el-button type="primary">查 询</el-button>
      <el-button type="warning">重 置</el-button>
    </div>
    <div class="card" style="margin-bottom: 5px ">
      <el-button type="primary">新 增</el-button>
      <el-button type="warning">批量删除</el-button>
      <el-button type="info">导入</el-button>
      <el-button type="success">导出</el-button>
    </div>
    <div class="card" style="margin-bottom: 5px">
      <el-table :data="data.tableData" stripe>
        <el-table-column label="日期" prop="date" />
        <el-table-column label="名称" prop="name" />
        <el-table-column label="地址" prop="address" />
      </el-table>
        <div style="margin-top: 10px">
          <el-pagination v-model:current-page="data.pageNum"
                         v-model:page-size="data.pageSize"
                         :page-sizes="[5, 10, 15, 20]"
                         background
                         layout="total, sizes, prev, pager, next, jumper"
                         :total="data.total"/>
        </div>
    </div>
  </div>
</template>

<script setup>
import  { reactive } from "vue";

const data = reactive( {
  name:null,
  tableData: [
    { id: 1 , date:'2025-02-06',name: 'dd1大大', address: '江西赣州'},
    { id: 2 , date:'2025-02-06',name: 'dd2大大', address: '江西赣州'},
    { id: 3 , date:'2025-02-06',name: 'dd3大大', address: '江西赣州'},
  ],
  pageNum: 1,
  pageSize: 10,
  total: 47,
})
</script>

<style scoped>


</style>