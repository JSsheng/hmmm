<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="Subject-title">
        <div class="title-left">
          <span class="iop">学科名称</span>
          <el-input class="left_input1"  v-model.trim="query.directoryName" ></el-input>
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
          <el-button @click="Addlist" type="success" icon="el-icon-edit" class="button_2" >新增目录</el-button>
          <el-dialog
              title="新增目录"
              :visible.sync="dialogVisible"
              width="30%"
              >
            <template>
              <span class="btn">所属学科</span>
              <el-select v-model="addList.subjectID" placeholder="请选择">
                <el-option
                  v-for="item in lists"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
              <br>
              <br>
              <div>
               <span class="op">*</span>
              <span class="btn">目录名称</span>
              <el-input style="width:60%" v-model.trim="addList.directoryName" placeholder="请输入内容"></el-input>
             </div>
             </template>
              <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="YesAddList">确 定</el-button>
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
        prop="directoryName"
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
          <el-button type="text"  @click="blllllll(row)">
          {{row.state === 1 ? '禁用' : '启用'}}
          </el-button>
          <el-button @click="SetXiugag(row)"  type="text" :disabled="row.state === 0" >修改</el-button>
      <el-dialog
         title="修改目录"
         :visible.sync="SetdialogVisible"
         width="30%"
         >
       <template>
         <span class="btn">所属学科</span>
         <el-select v-model="modifyDirectory.subjectID" placeholder="请选择">
           <el-option
             v-for="item in lists"
             :key="item.value"
             :label="item.label"
             :value="item.value">
           </el-option>
         </el-select>
         <br>
         <br>
         <div>
          <span class="op">*</span>
         <span class="btn">目录名称</span>
         <el-input style="width:60%" v-model.trim="modifyDirectory.directoryName" placeholder="请输入内容"></el-input>
        </div>
        </template>
         <span slot="footer" class="dialog-footer">
           <el-button @click="dialogVisible = false">取 消</el-button>
           <el-button type="primary" @click="YEsSetlist">确 定</el-button>
         </span>
       </el-dialog>
          <el-button @click="delxiugai(row.id)" type="text"  :disabled="row.state === 0" >删除</el-button>
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
import { list, simple, changeState, remove, update, add } from '../../api/hmmm/directorys'
export default {
  data () {
    return {
      SetInput: '',
      SetdialogVisible: false,
      DisabledType: false,
      dialogVisible: false,
      tableData: [],
      table: 1,
      Statevalue: '',
      left_input: '',
      state: [
        {
          value: 1,
          label: '启用'
        }, {
          value: 0,
          label: '禁用'
        }
      ],
      query: {
        page: 1,
        pagesize: 10,
        subjectID: '',
        directoryName: '',
        state: null
      },
      addList: {
        directoryName: '',
        subjectID: ''
      },
      lists: [],
      modifyDirectory: {
        subjectID: '',
        directoryName: '',
        id: ''
      }
    }
  },
  created () {
    this.Subjectstate()
    dayjs()
    this.sjiandanList()
    // console.log(this.type)
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
      this.query.directoryName = ''
      this.query.state = ''
    },
    SearchList () {
      this.Subjectstate()
    },
    async sjiandanList () {
      const { data } = await simple(this.subjectName)
      this.lists = data
      // console.log(data)
    },
    async blllllll (row) {
      // console.log(row.state)
      try {
        await changeState({ id: row.id, state: +(!row.state) })
        this.Subjectstate()
        this.$message.success('更新成功')
      } catch (error) {
        this.$message.error('更新失败')
      }
    },
    async delxiugai (id) {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        await remove({ id: id })
        this.Subjectstate()
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
    },
    SetXiugag (row) {
      this.modifyDirectory.id = row.id
      this.modifyDirectory.subjectID = row.subjectID
      this.SetdialogVisible = true
      this.modifyDirectory.directoryName = row.directoryName
    },
    async  YEsSetlist () {
      try {
        await update(this.modifyDirectory)
        this.SetdialogVisible = false
        this.Subjectstate()
        this.$message.success('修改成功')
      } catch (error) {
        this.$message.error('修改失败')
      }
    },
    Addlist () {
      this.dialogVisible = true
    },
    async YesAddList () {
      try {
        await add(this.addList)
        this.dialogVisible = false
        this.addList = {
          subjectID: '',
          directoryName: ''
        }
        this.Subjectstate()
        this.$message.success('添加成功')
      } catch (error) {
        this.$message.error('添加失败')
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

    .op{
      color: red  ;
    }

    .btn{
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
