<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="hmmm_father">
        <div class="left">
        <el-input style="width:250px;margin-right:10px" placeholder="根据用户名搜索"></el-input>
        <el-button>清除</el-button>
        <el-button type="primary">搜索</el-button>
      </div>
      <div class="right">
        <el-button type="success" icon="el-icon-edit">新增权限组</el-button>
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
          ref="multipleTable"
          :data="tableData"
          tooltip-effect="dark"
          style="width: 100%"
          >
          <el-table-column
            type="selection"
            width="55">
          </el-table-column>
          <el-table-column
            label="用户名"
            prop="title"
            width="600">
          </el-table-column>
          <el-table-column
            label="日期"
            sortable
            width="600">
            <template slot-scope="{row}">
              {{row.update_date}}
            </template>
          </el-table-column>
          <el-table-column
            label="操作"
            width="150"
            show-overflow-tooltip>
            <template>
              <el-button @click="ISeditDialogVisible" style="color:#45b1ff;background-color:#ecf5ff;" icon="el-icon-edit" circle></el-button>
              <el-button  style="color:#f56c6c;background-color:#fef0f0;"  icon="el-icon-delete" circle></el-button>
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
      :page-size="permissions.pagesize"
      :page-sizes="[5, 10, 20, 50]"
      layout="prev, pager,next,sizes,total, jumper"
      :total="table">
    </el-pagination>
    </div>
    <el-dialog
      title="编辑权限组"
      :visible.sync="editDialogVisible"
      width="30%"
      >
      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
    </div>
  </div>
</template>

<script>
import { list } from '../../api/base/permissions'
export default {
  data () {
    return {
      editDialogVisible: false,
      tableData: [],
      table: 0,
      permissions: {
        page: 1,
        pagesize: 10,
        keyword: ''
      }
    }
  },
  created () {
    this.permissionsList()
  },
  methods: {
    async permissionsList () {
      const { data } = await list(this.permissions)
      this.tableData = data.list
      this.table = data.counts
      // console.log(data)
    },
    handleSiz (val) {
      this.permissions.page = 1
      this.permissions.pagesize = val
      this.permissionsList()
    },
    Currentchange (val) {
      this.permissions.page = val
      this.permissionsList()
    },
    ISeditDialogVisible () {
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

    .hmmm_father{
      display: flex;
    justify-content: space-between;
    }

    .message{
      margin-top: 10px;
    }

  }
}
</style>
