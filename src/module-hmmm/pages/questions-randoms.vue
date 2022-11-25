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
        prop="id"
        label="编号"
        width="220">
      </el-table-column>
      <el-table-column
        prop="questionType"
        label="题型"
        width="80">
      </el-table-column>
      <el-table-column
        width="220"
        label="题目编号">
        <template slot-scope="{row}">
          <p v-for="item in row.questionIDs" :key="item.id + Math.random()">
            {{item.number}}
          </p>
        </template>
      </el-table-column>
      <el-table-column
        label="录入时间">
        <template slot-scope="{row}">
            {{ $dayjs(row.addTime).format('YYYY-MM-DD  HH:mm:ss')  }}
          </template>
      </el-table-column>
      <!-- progressOfAnswer -->
      <el-table-column
       prop="totalSeconds"
        label="答题时间(s)">
      </el-table-column>
      <el-table-column
        prop="accuracyRate"
        label="正确率(%)">
      </el-table-column>
      <el-table-column
        prop="userName"
        label="录入人">
      </el-table-column>
       <el-table-column
        label="操作"
        width="180">
        <el-button type="danger" icon="el-icon-delete" circle></el-button>
      <template>
        </template>
      </el-table-column>
    </el-table>
  </template>
  <div class="block">
    <!-- @size-change="handleSizeChange"
    @current-change="handleCurrentChange" -->
    <el-pagination
      @size-change="handleSiz"
      @current-change="Currentchange"
      :page-size="query.pagesize"
      :page-sizes="[5, 10, 20, 50]"
      layout="prev, pager,next,sizes,total, jumper"
      :total="table">
    </el-pagination>
  </div>
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs'
import { randoms } from '../../api/hmmm/questions'
export default {
  data () {
    return {
      table: 1,
      left_input: '',
      items: [],
      query: {
        page: 1,
        pagesize: 10,
        keyword: ''
      }
    }
  },
  created () {
    this.Subjectlist()
    dayjs()
  },
  methods: {
    async Subjectlist () {
      const { data } = await randoms(this.query)
      this.items = data.items
      this.table = data.counts
      // console.log(data)
    },
    handleSiz (val) {
      // console.log(val)
      this.query.page = 1
      this.query.pagesize = val
      this.Subjectlist()
    },
    Currentchange (val) {
      this.query.page = val
      this.Subjectlist()
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
