<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <span class="iop">学科名称</span>
          <el-input class="left_input" v-model.trim="query.subjectName" ></el-input>
          <el-button @click="DeleatInput">清除</el-button>
          <el-button @click="SearchList" type="primary">搜索</el-button>
        </div>
        <div class="title-right">
          <el-button @click="dialogVisible = true" type="success" icon="el-icon-edit" class="button_2" >新增学科</el-button>
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
        label="创建日期"
        width="220">
        <template slot-scope="{row}">
          {{ $dayjs(row.addDate).format('YYYY-MM-DD  HH:mm:ss')  }}
        </template>
      </el-table-column>
      <el-table-column
        label="前台是否显示">
        <template slot-scope="{row}">
          {{row.isFrontDisplay === 1 ? '是' : '否 '}}
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
        <template  slot-scope="{row}">
          <router-link to="/subjects/directorys"><el-button type="text">学科分类</el-button></router-link>
          <router-link to="/subjects/tags"><el-button type="text">学科标签</el-button></router-link>
            <el-button @click="SetList(row)" type="text">修改</el-button >
            <el-button @click="delLift(row)" type="text">删除</el-button >
          <template>
            <el-popconfirm
                confirm-button-text='好的'
                cancel-button-text='不用了'
                icon="el-icon-info"
                icon-color="red"
                title="这是一段内容确定删除吗？"
              >
             <el-button slot="reference">删除</el-button>
              </el-popconfirm>
          </template>
        </template>
      </el-table-column>
    </el-table>
  </template>
  <div class="block">
    <el-pagination
      @size-change="handleSiz"
      @current-change="Currentchange"
      :page-size="query.pagesize"
      :page-sizes="[5, 10, 20, 50]"
      layout="prev, pager,next,sizes,total, jumper"
      :total="table">
    </el-pagination>
  </div>
  <!--! 新增学科弹框 -->
<el-dialog
  title="新增学科"
  :visible.sync="dialogVisible"
  width="25%"
  >
  <span class="o">*</span>
  <span class="p">学科名称</span>
  <el-input style="width:60%" v-model="curForm.subjectName" placeholder="请输入学科名称"></el-input>
  <br/>
  <br/>
  <br/>
<div>
  <span>是否显示</span>
  <el-switch
    v-model="curForm.isFrontDisplay"
    active-color="#13ce66"
    inactive-color="#ff4949">
  </el-switch>
</div>
  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="addCurFrom">确 定</el-button>
  </span>
</el-dialog>
<!--!  修改弹框 -->
<el-dialog
  title="修改学科"
  :visible.sync="subjiectInput"
  width="25%"
  >
  <span class="o">*</span>
  <span class="p">学科名称</span>
  <el-input style="width:60%" v-model.trim="curForm.subjectName" placeholder="请输入学科名称"></el-input>
  <br/>
  <br/>
  <br/>
<div>
  <span>是否显示</span>
  <el-switch
    v-model="curForm.isFrontDisplay"
    active-color="#13ce66"
    inactive-color="#ff4949">
  </el-switch>
</div>
  <span slot="footer" class="dialog-footer">
    <el-button @click="subjiectInput = false">取 消</el-button>
    <el-button type="primary" @click="YesSetsubjiectInput">确 定</el-button>
  </span>
</el-dialog>
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs'
import { list, update, add, remove } from '../../api/hmmm/subjects'
export default {
  data () {
    return {
      id: '',
      curForm: {
        isFrontDisplay: null,
        id: '',
        subjectName: ''
      },
      subjiectInput: false,
      dialogVisible: false,
      table: 1,
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
    dayjs()
    // console.log(dayjs())
  },
  methods: {
    async Subjectlist () {
      const { data } = await list(this.query)
      this.items = data.items
      this.table = data.counts
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
    // 清空input框
    DeleatInput () {
      this.query.subjectName = ''
    },
    SearchList () {
      this.Subjectlist()
    },
    SetList (row) {
      // console.log(row)
      this.subjiectInput = true
      this.curForm.subjectName = row.subjectName
      this.curForm.id = row.id
      this.curForm.isFrontDisplay = !!row.isFrontDisplay
      // console.log(this.curForm.isFrontDisplay)
    },
    async YesSetsubjiectInput () {
      try {
        // await update({ ...this.curForm, isFrontDisplay: Number(this.curForm.isFrontDisplay) })
        await update({ ...this.curForm, isFrontDisplay: +this.curForm.isFrontDisplay })
        this.$message.success('成功')
        this.subjiectInput = false
        this.Subjectlist()
      } catch (error) {
        this.$message.error('失败')
      }
    },
    async addCurFrom () {
      try {
        const addFrom = this.curForm
        await add(addFrom)
        this.$message.success('添加成功')
        this.dialogVisible = false
        this.curForm.subjectName = ''
        this.Subjectlist()
      } catch (error) {
        this.$message.error('添加失败')
      }
    },
    // 删除
    async delLift (row) {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        await remove({ id: row.id })
        // console.log('qqqqq')
        this.Subjectlist()
        this.$message({
          type: 'success',
          message: '删除成功'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '取消删除'
        })
      })
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

    .o{
      color: red;
    }

    .p{
      font-weight: 700;
      margin-right: 10px;
    }
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
  .block{
    margin-top: 10px;
  }
}
</style>
