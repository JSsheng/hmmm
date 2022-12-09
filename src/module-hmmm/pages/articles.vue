<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <span class="iop">关键字</span>
          <el-input class="left_input"  placeholder="根据文章标题搜索" v-model.trim="left_input" ></el-input>
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
      :data="items"
      style="width: 100%">
      <el-table-column
        type="index"
        label="序号"
        width="80">
      </el-table-column>
      <el-table-column
        prop="title"
        label="文章标题"
        width="180">
      </el-table-column>
      <el-table-column
        prop="visits"
        label="阅读数">
      </el-table-column>
      <el-table-column
        prop="username"
        label="录入人">
      </el-table-column>
      <el-table-column
        label="录入时间">
        <template slot-scope="{row}">
            {{ $dayjs(row.createTime).format('YYYY-MM-DD  HH:mm:ss')  }}
        </template>
      </el-table-column>
      <el-table-column
        prop="state"
        label="状态">
      </el-table-column>
       <el-table-column
        label="操作"
        width="280">
        <template>
          <el-button @click="ISpreview" type="text" >预览</el-button>
          <el-button type="text" >禁用</el-button >
          <el-button type="text" >修改</el-button >
          <el-button type="text" >删除</el-button >
        </template>
      </el-table-column>
      </el-table>

  <div class="block">
    <!-- @size-change="handleSizeChange"
    @current-change="handleCurrentChange" -->
    <el-pagination
      @size-change="handleSiz"
      @current-change="Currentchange"
      :page-size="query.pagesize"
      :page-sizes="[1,5, 10, 20, 50]"
      layout="prev, pager,next,sizes,total, jumper"
      :total="table">
    </el-pagination>
  </div>

  <el-dialog
      title="编辑用户"
      :visible.sync="editDialogVisible"
      width="30%"
      >
      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </template>
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs'
import { list } from '../../api/hmmm/articles'
export default {
  data () {
    return {
      editDialogVisible: false,
      Statevalue: '',
      table: 0,
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
      items: [],
      query: {
        page: 1,
        pagesize: 10,
        keyword: '',
        state: ''
      }
    }
  },
  created () {
    this.Subjectlist()
    dayjs()
  },
  methods: {
    async Subjectlist () {
      const { data } = await list(this.query)
      this.items = data.items
      this.table = data.counts
      // console.log(data)
    },
    handleSiz (val) {
      this.query.page = 1
      this.query.pagesize = val
      this.Subjectlist()
    },
    Currentchange (val) {
      this.query.page = val
      this.Subjectlist()
    },
    ISpreview () {
      this.editDialogVisible = true
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
