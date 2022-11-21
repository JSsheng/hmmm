<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <span class="iop">学科名称</span>
          <el-input class="left_input" v-model.trim="left_input" ></el-input>
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
      :data="items"
      style="width: 100%">
      <el-table-column
        type="index"
        label="序号"
        width="80">
      </el-table-column>
      <el-table-column
        prop="subjectName"
        label="学科名称"
        width="180">
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
        label="前台是否显示">
        <template slot-scope="{row}">
          {{row.totals === 1 ? '是' : '否 '}}
        </template>
      </el-table-column>
      <el-table-column
        prop="twoLevelDirectory"
        label="二级目录">
      </el-table-column>
      <el-table-column
        prop="tags"
        label="标签">
      </el-table-column>
      <el-table-column
        prop="totals"
        label="题目数量">
      </el-table-column>
       <el-table-column
        label="操作"
        width="280">
        <template>
          <el-button type="primary">学科分类</el-button>
          <el-button type="primary">学科标签</el-button>
          <el-button type="primary">修改</el-button >
          <el-button type="primary">删除</el-button >
        </template>
      </el-table-column>
    </el-table>
  </template>
    </div>
  </div>
</template>

<script>
import { list } from '../../api/hmmm/subjects'
export default {
  data () {
    return {
      left_input: '',
      table: '',
      query: {
        page: 1,
        pagesize: 10,
        subjectName: ''
      },
      items: []
    }
  },
  created () {
    this.Subjectlist()
  },
  methods: {
    async Subjectlist () {
      const { data } = await list(this.query)
      this.items = data.items
      this.table = data.counts
      console.log(this.items)
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
  }
  .title-right{
    .button_2{
      width: 140px;
    }
  }
    }
  }
}
</style>
