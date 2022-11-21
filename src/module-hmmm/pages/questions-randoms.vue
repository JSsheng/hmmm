<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <span class="iop">关键字</span>
          <el-input class="left_input"  placeholder="根据文章标题搜索" v-model.trim="left_input" ></el-input>

        </div>
        <div class="title-right">
          <el-button>清除</el-button>
          <el-button type="primary">搜索</el-button>
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
        prop="title"
        label="编号"
        width="80">
      </el-table-column>
      <el-table-column
        prop="title"
        label="题型"
        width="180">
      </el-table-column>
      <el-table-column
        prop="visits"
        label="题目编号">
      </el-table-column>
      <el-table-column
        prop="username"
        label="录入时间">
      </el-table-column>
      <el-table-column
        prop="createTime"
        label="答题时间(s)">
      </el-table-column>
      <el-table-column
        prop="state"
        label="正确率(%)">
      </el-table-column>
      <el-table-column
        prop="state"
        label="录入人">
      </el-table-column>
       <el-table-column
        label="操作"
        width="180">
      <template>

        </template>
      </el-table-column>
    </el-table>
  </template>
    </div>
  </div>
</template>

<script>
import { list } from '../../api/hmmm/questions'
export default {
  data () {
    return {
      table: '',
      left_input: '',
      items: [],
      query: {
        page: '1',
        pagesize: '10',
        keyword: ''
      }
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

    .i{
      color: #66b1ff;
      margin-right: 10px;
    }
  }
}
</style>
