<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <el-input class="left_input1" placeholder="根具用户名搜索" v-model.trim="left_input" ></el-input>
          <el-button>清除</el-button>
          <el-button type="primary">搜索</el-button>
        </div>
        <div class="title-right">
          <el-button type="success" icon="el-icon-edit" class="button_2" >新增学科</el-button>
        </div>
      </div>
      <div class="message" >
        <el-alert
          type="info"
          :closable="false"
          show-icon>
          <template>
            数据一共{{table}}条
          </template>
        </el-alert>
      </div>
     <template>
      <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="id"
        label="序号"
        width="180">
      </el-table-column>
      <el-table-column
        prop="email"
        label="邮箱"
        width="180">
      </el-table-column>
      <el-table-column
        prop="phone"
        label="联系电话">
      </el-table-column>
      <el-table-column
        prop="username"
        label="用户名">
      </el-table-column>
      <el-table-column
        prop="permission_group_title"
        label="权限组名称">
      </el-table-column>
      <el-table-column
        prop="role"
        label="角色">
      </el-table-column>
      <el-table-column
        label="操作">
        <template>
          <el-button type="primary" icon="el-icon-edit" circle></el-button>
        </template>
      </el-table-column>
    </el-table>
     </template>
    </div>
  </div>
</template>

<script>
import { list } from '../../api/base/users'
export default {
  data () {
    return {
      left_input: '',
      tableData: [],
      query: {
        page: '1',
        pagesize: '10',
        keyword: '',
        disabled: ''
      },
      table: ''
    }
  },
  created () {
    this.GetUsersList()
  },
  methods: {
    async GetUsersList () {
      const { data } = await list(this.query)
      this.tableData = data.list
      this.table = data.counts
      console.log(data)
    }
  }
}
</script>

<style scoped lang='less'>
.container{
  padding: 10px;
  .Subject-father{
    background-color: #fff;
    min-height: 200px;
    width: 100%;
    padding: 15px;

    .Subject-title{
      display: flex;
      justify-content:space-between;
      margin-bottom: 15px;
      .title-left{
    .left_input1{
      width: 200px;
      margin-right: 10px;
    }
  }
  .title-right{
    .button_2{
      width: 140px;
    }
  }
    }
    .message{
      margin-bottom: 10px;
    }

  }
}
</style>
