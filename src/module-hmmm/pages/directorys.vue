<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <span class="iop">学科名称</span>
          <el-input class="left_input1"  v-model.trim="left_input" ></el-input>
          <span class="iop">状态</span>
          <template>
           <el-select class="left_input" v-model="Statevalue" placeholder="请选择">
             <el-option
               v-for="item in state"
               :key="item.value"
               :label="item.label"
               :value="item.value">
             </el-option>
           </el-select>
          </template>
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
        type="index"
        label="序号"
        width="80">
      </el-table-column>
      <el-table-column
        prop="subjectName"
        label="所属学科"
        width="180">
      </el-table-column>
      <el-table-column
        prop="directoryName"
        label="目录名称">
      </el-table-column>
      <el-table-column
        prop="username"
        label="创建者">
      </el-table-column>
      <el-table-column
        prop="addDate"
        label="创建日期">
      </el-table-column>
      <el-table-column
        prop="totals"
        label="面试题数量">
      </el-table-column>
      <el-table-column
        label="状态">
        <template slot-scope="{row}">
          {{ row.state === 1 ? '已启用' : '已禁用'}}
        </template>
      </el-table-column>
      <el-table-column
        label="操作">
        <template>
          <span class="i">禁用</span>
          <span class="i">修改</span>
          <span class="i">删除</span>
        </template>
      </el-table-column>
    </el-table>
  </template>
    </div>
  </div>
</template>

<script>
import { list } from '../../api/hmmm/directorys'
export default {
  data () {
    return {
      tableData: [],
      table: '',
      Statevalue: '',
      left_input: '',
      state: [
        {
          value: '选项1',
          label: '启用'
        }, {
          value: '选项2',
          label: '禁用'
        }
      ],
      query: {
        page: 1,
        pagesize: 10,
        subjectID: '',
        directoryName: '',
        state: ''
      }
    }
  },
  created () {
    this.Subjectstate()
  },
  methods: {
    async Subjectstate () {
      const search = this.query
      if (search.subjectID === '') {
        delete search.subjectID
      }
      const { data } = await list(search)
      this.tableData = data.items
      this.table = data.counts
      // console.log(data)
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
    .iop{
      color: #606266;
      font-weight: 700;
      margin-right: 5px;
    }
    .left_input{
      width: 270px;
      margin-right: 5px;
    }

    .left_input1{
      width: 270px;
      margin-right: 35px;
    }
  }
  .title-right{
    .button_2{
      width: 140px;
    }
  }
    }

    .i{
      color: #66b1ff;
      margin-right: 10px;
    }
  }
}
</style>
