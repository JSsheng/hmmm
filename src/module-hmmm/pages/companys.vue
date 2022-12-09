<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="hmmm_feather">
        <div class="top">
          <span class="i">标签名称</span>
          <el-input v-model="tabList.TabName" style="width:350px;margin-right:10px"></el-input>
          <span class="i">城市</span>
          <template>
            <el-select @change="secarhChange" style="width:350px;margin-right:10px" v-model="tabList.city" placeholder="请选择">
              <el-option
                v-for="item in Cityoptions"
                :key="item"
                :label="item"
                :value="item">
              </el-option>
            </el-select>
          </template>
          <span class="i">地区</span>
          <template>
            <el-select  style="width:350px;margin-right:10px" v-model="tabList.region" placeholder="请选择">
              <el-option
                v-for="item in Oregionptions"
                :key="item"
                :label="item"
                :value="item">
              </el-option>
            </el-select>
          </template>
          <span class="i">企业简称</span>
          <el-input v-model="tabList.enterprise" style="width:350px"></el-input>
        </div>
        <div class="next">
         <div>
          <span class="i">状态</span>
          <template>
            <el-select style="width:350px;margin-right:30px" v-model="tabList.state" placeholder="请选择">
              <el-option
                v-for="item in stateOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <el-button @click="delList">清除</el-button>
          <el-button @click="searchFrom" type="primary">搜索</el-button>
         </div>
         <div>
          <el-button @click="up" type="success" icon="el-icon-edit">新增用户</el-button>
          <el-dialog
            :title="name === '新增' ? '新增用户' : '编辑用户'"
            :visible.sync="UserdialogVisible"
            width="50%">
            <div>
              <span class="p">*</span>
              <span  class="i">企业名称</span>
              <el-input v-model="addList.shortName" style="width:60%;margin-bottom: 20px;"></el-input>
            </div>
            <div>
              <span class="p" >*</span>
              <span class="i">所属公司</span>
              <el-input v-model="addList.company" style="width:60%;margin-bottom: 20px;"></el-input>
            </div>
            <div>
               <a style="display:block;margin-bottom:20px;" href="https://www.tianyancha.com">https://www.tianyancha.com在此可查询所属公司全称及简称）</a>
            </div>
            <div>
              <span class="p" >*</span>
              <span class="i">城市地区</span>
              <el-select @change="secarhChange" style="margin-bottom: 20px;margin-right: 10px;" v-model="addList.city" placeholder="请选择">
                <el-option
                  v-for="item in Cityoptions"
                  :key="item"
                  :label="item"
                  :value="item">
                </el-option>
              </el-select>
              <el-select v-model="addList.province" placeholder="请选择">
                  <el-option
                    v-for="item in Oregionptions"
                    :key="item"
                    :label="item"
                    :value="item">
                  </el-option>
              </el-select>
            </div>
            <div>
              <span class="p" >*</span>
              <span class="i">方向(企业标签)</span>
              <el-input v-model="addList.tags" style="width:60%;margin-bottom: 20px;"></el-input>
            </div>
            <div>
              <el-form ref="form" :model="form" label-width="80px">
                <!-- <span class="p" >*</span> -->
                <el-form-item label="备注">
                <el-input v-model="addList.remarks" style="width:60%;"  type="textarea"></el-input>
              </el-form-item>
              </el-form>
            </div>
            <span slot="footer" class="dialog-footer">
              <el-button @click="UserdialogVisible = false">取 消</el-button>
              <el-button type="primary" @click="addUser">确 定</el-button>
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
               prop="id"
               label="序号"
               width="80">
             </el-table-column>
             <el-table-column
               prop="number"
               label="企业编号"
               width="250">
             </el-table-column>
             <el-table-column
               prop="shortName"
               label="企业简称"
               width="110">
             </el-table-column>
             <el-table-column
               prop="tags"
               label="标签"
               width="80">
             </el-table-column>
             <el-table-column
               prop="creatorID"
               label="创建者"
               width="80">
             </el-table-column>
             <el-table-column
               label="创建日期"
               width="220">
               <template slot-scope="{row}">
                {{ $dayjs(row.addDate).format('YYYY-MM-DD  HH:mm:ss')  }}
               </template>
             </el-table-column>
             <el-table-column
               prop="remarks"
               label="备注">
             </el-table-column>
             <el-table-column
               prop="state"
               label="状态">
               <template slot-scope="{row}">
                {{row.state === 1 ? '启用' : '禁用'}}
               </template>
             </el-table-column>
             <el-table-column
               prop="address"
               label="操作">
               <template slot-scope="{row}">
                <el-button @click="ISeditDialogVisible(row)" style="color:#45b1ff;background-color:#ecf5ff;" icon="el-icon-edit" circle></el-button>
                <el-button @click="alterState(row)" style="color:#e6a36f;background-color:#fdf6ec;" :icon="row.state === 1 ? 'el-icon-close' : 'el-icon-check'" circle></el-button>
                <el-button @click="removeList(row.id)"  style="color:#f56c6c;background-color:#fef0f0;"  icon="el-icon-delete" circle></el-button>
               </template>
             </el-table-column>
           </el-table>
  <div class="block">
    <!-- @size-change="handleSizeChange"
    @current-change="handleCurrentChange" -->
    <el-pagination
      @size-change="handleSiz"
      @current-change="Currentchange"
      :page-size=" tabList.pagesize"
      :page-sizes="[5, 10, 20, 50]"
      layout="prev, pager,next,sizes,total, jumper"
      :total="table">
    </el-pagination>
  </div>
        </template>

      </div>

    </div>

  </div>
</template>

<script>
import dayjs from 'dayjs'
import { list, add, remove, disabled, update } from '../../api/hmmm/companys'
import { provinces, citys } from '../../api/hmmm/citys'
export default {
  data () {
    return {
      name: '',
      form: {},
      addList: {
        shortName: '',
        company: '',
        city: '',
        province: '',
        tags: '',
        remarks: '',
        isFamous: true,
        id: '',
        number: ''
      },
      // id: '',
      UserdialogVisible: false,
      tabList: {
        page: 1,
        pagesize: 10,
        TabName: '',
        city: '',
        region: '',
        enterprise: '',
        state: null
      },
      editDialogVisible: false,
      // 市级下拉框
      Cityoptions: [],
      // 地区下拉框
      Oregionptions: [],
      // 状态下拉框
      stateOptions: [
        {
          value: '1',
          label: '启用'
        },
        {
          value: '2',
          label: '禁用'
        }
      ],
      tableData: [],
      options: [],
      table: 0
    }
  },
  created () {
    this.CompanysList()
    dayjs()
    this.InitCity()
  },
  methods: {
    async CompanysList () {
      const from = {}
      for (const key in this.tabList) {
        if (this.tabList[key]) {
          from[key] = this.tabList[key]
        }
      }
      const { data } = await list(from)
      this.tableData = data.items
      this.table = data.counts
      // console.log(data)
    },
    handleSiz (val) {
      this.tabList.page = 1
      this.tabList.pagesize = val
      this.CompanysList()
    },
    Currentchange (val) {
      this.tabList.page = val
      this.CompanysList()
    },
    ISeditDialogVisible (row) {
      // this.addList.id = row.id
      // console.log(row)
      this.name = '编辑'
      this.UserdialogVisible = true
      this.addList = {
        shortName: row.shortName,
        company: row.company,
        city: row.city,
        province: row.province,
        tags: row.tags,
        remarks: row.remarks,
        id: row.id,
        number: row.number,
        addDate: row.addDate,
        state: row.state,
        isFamous: true
      }
    },
    delList () {
      this.tabList = {
        TabName: '',
        city: '',
        region: '',
        enterprise: '',
        value: ''
      }
    },
    InitCity () {
      this.Cityoptions = provinces()
    },
    secarhChange (val) {
      this.Oregionptions = citys(val)
    },
    // 筛选
    searchFrom () {
      this.CompanysList()
    },
    up () {
      this.name = '新增'
      this.UserdialogVisible = true
    },
    // 添加用户
    async addUser () {
      if (this.name === '新增') {
        try {
          await add(this.addList)
          this.UserdialogVisible = false
          this.addList = {
            shortName: '',
            company: '',
            city: '',
            province: '',
            tags: '',
            remarks: '',
            isFamous: true,
            id: '',
            number: ''
          }
          this.CompanysList()
          this.$message.success('添加成功')
        } catch (error) {
          this.$message.error('添加失败')
        }
      } else {
        try {
          // console.log(this)
          await update(this.addList)
          this.addList = {
            shortName: '',
            company: '',
            city: '',
            province: '',
            tags: '',
            remarks: '',
            isFamous: true,
            id: '',
            number: ''
          }
          this.CompanysList()
          this.UserdialogVisible = false
          this.$message.success('修改成功')
        } catch (error) {
          this.$message.error('修改失败')
          this.UserdialogVisible = false
          this.addList = {
            shortName: '',
            company: '',
            city: '',
            province: '',
            tags: '',
            remarks: '',
            isFamous: true,
            id: '',
            number: ''
          }
        }
      }
    },
    // 删除
    removeList (id) {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        await remove({ id: id })
        this.CompanysList()
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
    async alterState ({ id, state }) {
      console.log(state)
      try {
        await disabled({ id, state: +(!state) })
        this.CompanysList()
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

    .next{
      margin-top: 10px;
      display: flex;
      justify-content: space-between;
    }

    .message{
      margin-top: 10px;
    }

    .p{
      color: red;
      margin-right: 5px;
    }
    .i{
      font-weight: 700;
      margin-right: 10px;
    }
  }
}
</style>
