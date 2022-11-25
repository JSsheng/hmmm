
<style scoped lang='less'></style>
<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <span class="iop">学科名称</span>
          <el-input class="left_input1" v-model.trim="query.tagName" ></el-input>
          <span class="iop">状态</span>
          <template>
           <el-select class="left_input" v-model="query.state" placeholder="请选择">
             <el-option
               v-for="item in state"
               :key="item.value"
               :label="item.label"
               :value="item.value">
             </el-option>
           </el-select>
          </template>
          <el-button @click="DeleatInput">清除</el-button>
          <el-button @click="SearchList" type="primary">搜索</el-button>
        </div>
        <div class="title-right">
          <el-button @click="xingzengbiaoqian" type="success" icon="el-icon-edit" class="button_2" >新增标签</el-button>
          <el-dialog
             title="提示"
             :visible.sync="dialogVisible"
             width="30%"
             >
             <span class="btn">所属学科</span>
             <el-select v-model="addDirectory.subjectID" placeholder="请选择">
               <el-option
                 v-for="item in lists"
                 :key="item.value"
                 :label="item.label"
                 :value="item.value">
               </el-option>
             </el-select>
             <br>
             <br>
             <br>
             <span class="op">*</span>
             <span class="btn">目录名称</span>
             <el-input style="width:60%" v-model="addDirectory.tagName" placeholder="请输入内容"></el-input>
             <span slot="footer" class="dialog-footer">
               <el-button @click="dialogVisible = false">取 消</el-button>
               <el-button type="primary" @click="YesAddDirectory">确 定</el-button>
             </span>
          </el-dialog>
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
        prop="tagName"
        label="目录名称">
      </el-table-column>
      <el-table-column
        prop="username"
        label="创建者">
      </el-table-column>
      <el-table-column
        label="创建日期"
        width="230">
        <template slot-scope="{row}">
          {{ $dayjs(row.addDate).format('YYYY-MM-DD  HH:mm:ss')  }}
        </template>
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
        <template slot-scope="{row}">
          <el-button @click="YesDisabled(row)" type="text" >
          {{row.state === 1 ? '禁用' : '启用'}}
          </el-button>
          <el-button @click="modification(row)" :disabled="row.state === 0" type="text" >修改</el-button>
          <el-dialog
              title="修改标签"
              :visible.sync="modificationDask"
              width="30%">
              <span class="btn">所属学科</span>
             <el-select v-model="MoifyList.subjectID" placeholder="请选择">
               <el-option
                 v-for="item in lists"
                 :key="item.value"
                 :label="item.label"
                 :value="item.value">
               </el-option>
             </el-select>
             <br>
             <br>
             <br>
             <span class="op">*</span>
             <span class="btn">表签名称</span>
             <el-input style="width:60%" v-model="MoifyList.tagName" placeholder="请输入内容"></el-input>
              <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="yesmodificationList">确 定</el-button>
              </span>
          </el-dialog>
          <el-button :disabled="row.state === 0" @click="remove(row)" type="text" >删除</el-button>
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
import { list, simple, remove, add, changeState, update } from '../../api/hmmm/tags'
export default {
  data () {
    return {
      modificationDask: false,
      disabled: false,
      mulumingcehng: '',
      lists: [],
      subjectName: '',
      dialogVisible: false,
      tableData: [],
      table: 1,
      Statevalue: '',
      state: [
        {
          value: 1,
          label: '已启用'
        }, {
          value: 0,
          label: '已禁用'
        }
      ],
      query: {
        page: 1,
        pagesize: 10,
        subjectID: '',
        tagName: '',
        state: ''
      },
      addDirectory: {
        subjectID: '',
        tagName: ''
      },
      MoifyList: {
        subjectID: '',
        tagName: '',
        id: ''
      }
    }
  },
  created () {
    this.Subjectstate()
    dayjs()
    this.addXinList()
  },
  methods: {
    async Subjectstate () {
      const search = this.query
      if (search.subjectID === '') {
        delete search.subjectID
      }
      if (search.tagName === '') {
        delete search.tagName
      }
      if (search.state === '') {
        delete search.state
      }
      const { data } = await list(search)
      this.tableData = data.items
      this.table = data.counts
      // console.log(data)
    },
    handleSiz (val) {
      this.query.page = 1
      this.query.pagesize = val
      this.Subjectstate()
    },
    Currentchange (val) {
      this.query.page = val
      this.Subjectstate()
    },
    DeleatInput () {
      this.query.tagName = ''
      this.query.state = ''
    },
    SearchList () {
      this.Subjectstate()
    },
    xingzengbiaoqian () {
      this.dialogVisible = true
    },
    async addXinList () {
      const { data } = await simple(this.subjectName)
      this.lists = data
      // console.log(data)
    },
    async remove (row) {
      try {
        await remove({ id: row.id })
        this.Subjectstate()
        this.$message.success('删除成功')
      } catch (error) {
        this.$message.error('删除失败')
      }
    },
    async YesAddDirectory () {
      try {
        await add(this.addDirectory)
        this.Subjectstate()
        this.dialogVisible = false
        this.addDirectory = {
          subjectID: '',
          tagName: ''
        }
        this.$message.success('添加成功')
      } catch (error) {
        this.$message.error('添加失败')
      }
    },
    async YesDisabled ({ id, state }) {
      try {
        await changeState({ id, state: +(!state) })
        this.Subjectstate()
        this.$message.success('更新成功')
      } catch (error) {
        this.$message.error('更新失败')
      }
    },
    modification (row) {
      console.log(row)
      this.MoifyList.subjectID = row.subjectID
      this.MoifyList.tagName = row.tagName
      this.MoifyList.id = row.id
      this.modificationDask = true
    },
    async yesmodificationList () {
      try {
        await update(this.MoifyList)
        this.addDirectory = {
          subjectID: '',
          directoryName: '',
          id: ''
        }
        this.Subjectstate()
        this.modificationDask = false
        this.$message.success('更新成功')
      } catch (error) {
        this.$message.error('更新失败')
      }
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

    .btn{
      font-weight: 700;
      margin-right: 10px;
    }
    .op{
      color: red;
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
