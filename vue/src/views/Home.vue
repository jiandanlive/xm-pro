<template>
  <div>
    <div style="margin-bottom: 20px">
      <RouterLink to="/manager/test">跳转到Test.vue</RouterLink> |
      <a href="/manager/test">通过a标签跳转</a>
    </div>
    <div style="margin-bottom: 20px">
      <el-button type="primary" @click="router.push('/manager/test')">push跳转到新的页面</el-button>
      <el-button type="primary" @click="router.replace('/manager/test')">replace跳转到新的页面</el-button>
    </div>

    <div style="margin-bottom: 20px">
      <el-button type="primary" @click="router.push('/manager/test?id=1&name=沥川')">路由传参id=1</el-button>
    </div>

    <div style="margin-bottom: 20px">
      <el-button type="primary" @click="router.push({path: '/manager/test', query:{id:2, name: '沥川'}})">路由传参id=1</el-button>
    </div>

    <div>
      <el-input clearable v-model="data.input" style="width: 240px" placeholder="请输入内容" :prefix-icon="Search" /> {{ data.input }}
      <el-input style="width: 200px;" :suffix-icon="Calendar"></el-input>
      <el-input type="textarea" v-model="data.description" style="width: 200px" placeholder="请输入一段描述"></el-input>
    </div>

    <div>
      <el-select
          clearable
          multiple
          value-key="id"
          v-model="data.value"
          placeholder="Select"
          size="large"
          style="width: 240px"
      >
        <el-option
            v-for="item in data.options"
            :key="item.id"
            :label="item.label"
            :value="item.name"
        />
      </el-select> {{ data.value }}
    </div>

    <div style="margin: 20px 0">
        <el-radio-group v-model="data.sex">
          <el-radio value="男">男</el-radio>
          <el-radio value="女">女</el-radio>
        </el-radio-group>  <span style="margin-left: 50px">{{ data.sex }}</span>

      <el-radio-group style="margin-left: 100px" v-model="data.tag" size="large">
        <el-radio-button label="我发布的内容" value="1" />
        <el-radio-button label="我点赞的内容" value="2" />
        <el-radio-button label="我收藏的内容" value="3" />
      </el-radio-group>
    </div>
    <div style="margin: 20px 0">
      <el-checkbox-group v-model="data.checkList">
        <el-checkbox v-for="item in data.options" :key="item.id" :value="item.name" :label="item.label" />
      </el-checkbox-group>
      <span style="margin: 20px 0">{{ data.checkList }}</span>
    </div>
    <div style="margin: 20px 0">
      <img src="@/assets/logo.svg" alt="" STYLE="width: 100px;">
      <el-image :src="img" style="width: 100px;margin-left: 100px" :preview-src-list="[img,'https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg']"/>
    </div>
    <div style="margin: 20px 0">
      <el-carousel height="400px" style="width: 550px">
        <el-carousel-item v-for="item in data.imgs" :key="item" style="">
          <img style="width: 100%;height: 400px" :src="item" alt="">
        </el-carousel-item>
      </el-carousel>
    </div>
    <div style="margin: 20px 0">
      <el-date-picker
          v-model="data.date"
          type="date"
          placeholder="请选择日期"
          format="YYYY-MM-DD"
          value-format="YYYY-MM-DD"
      /> {{ data.date }}
      <el-date-picker
          style="margin-left: 50px"
          v-model="data.date1"
          type="datetime"
          placeholder="请选择日期"
          format="YYYY-MM-DD HH:mm:ss"
          value-format="YYYY-MM-DD HH:mm:ss"
      /> {{ data.date1 }}
      <el-time-picker v-model="data.time" placeholder="Arbitrary time"
                      format="YYYY-MM-DD HH:mm:ss"
                      value-format="YYYY-MM-DD HH:mm:ss"
      />
      {{ data.time }}
      <el-date-picker
          style="margin-left: 50px"
          v-model="data.daterange"
          type="daterange"
          range-separator="到"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          format="YYYY-MM-DD"
          value-format="YYYY-MM-DD"
      />{{ data.daterange?.length ? data.daterange[0] : '' }} {{ data.daterange?.length ? data.daterange[1] : ''  }}
    </div>

    <div style="margin: 20px 0">
      <el-table :data="data.tableData" style="width: 100%" stripe border>
        <el-table-column prop="date" label="日期" width="180" />
        <el-table-column prop="name" label="名称" width="180" />
        <el-table-column prop="address" label="地址" />
        <el-table-column label="操作">

          <template #default="scope">
            <el-button type="primary" circle @click="edit(scope.row)">
              <el-icon><Edit /></el-icon>
            </el-button>
            <el-button type="danger" circle @click="del(scope.row.id)">
              <el-icon><Delete /></el-icon>
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <div style="padding: 10px 0" >
        <el-pagination
            v-model:current-page="data.currentPage"
            v-model:page-size="data.pageSize"
            :page-sizes="[5, 10, 15, 20]"
            background
            layout="total, sizes, prev, pager, next, jumper"
            :total="data.tableData.length"
        />
      </div>
    </div>
    <el-dialog v-model="data.dialogVisible" title="编辑行对象" width="500" >
      <div style="padding: 20px">
        <div style="margin-bottom: 10px">日期：{{ data.row.date }}</div>
        <div style="margin-bottom: 10px">名字：{{ data.row.name }}</div>
        <div >地址：{{ data.row.address }}</div>
      </div>
    </el-dialog>
  </div>
</template>

<script setup>

import {reactive} from "vue";
import {Calendar, Search} from "@element-plus/icons-vue";
import img from '@/assets/logo.svg'
import lun1 from '@/assets/lun1.jpg'
import lun2 from '@/assets/lun2.jpg'
import lun3 from '@/assets/lun3.jpg'
import router from "@/router/index.js";
import request from "@/utils/request.js";

const data = reactive({
  input: 123,
  description: '用于输入多行文本信息可缩放的输入框。 添加 type=\"textarea\" 属性来将 input 元素转换为原生的 textarea 元素。',
  value: '',
  options: [{id:1,label:'苹果',name:'苹果1'},{id:2,label:'香蕉',name: '香蕉'}, {id:3,label:'桃子',name: '桃子'},{id:4,label:'苹果',name: '苹果2'}],
  sex: '男',
  tag: '2',
  checkList: [],
  imgs:[lun1,lun2,lun3],
  date: [],
  date1: [],
  time:[],
  daterange:[],
  currentPage: 1,
  pageSize: 5,
  tableData: [
    { id: 1 , date:'2025-02-06',name: 'dd1大大', address: '江西赣州'},
    { id: 2 , date:'2025-02-06',name: 'dd2大大', address: '江西赣州'},
    { id: 3 , date:'2025-02-06',name: 'dd3大大', address: '江西赣州'},
  ],
  dialogVisible: false,
  row: null,
  employeeList: []
})

request.get('/employee/selectAll').then(res => {
  console.log(res)
  data.employeeList = res.data
})

const del = (id) => {
  alert("删除ID=" + id + "的数据")
}

const edit = (row) => {
  data.row = row
  data.dialogVisible = true
}

data.tableData = data.tableData.slice(0,5)
</script>