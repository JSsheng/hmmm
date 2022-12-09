<template>
  <div class='container'>
    <div class="Subject-father">
      <div class="hmmm_father">
        <div class="hmmm_folt">
          <div class="ppppp">说明：目前支持学科和关键字条件筛选</div>
          <div>
            <el-button type="success" icon="el-icon-edit"  >新增试题</el-button>
          </div>
        </div>
        <div class="hmmm_top">
          <!-- 学科 -->
          <template>
            <span>学科</span>
            <el-select style="width:18%" v-model="selects.subjectID" placeholder="请选择">
              <el-option
                v-for="item in subjectOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <!-- 二级目录 -->
          <template>
            <span>二级目录</span>
            <el-select style="width:18%" v-model="selects.catalogID" placeholder="请选择">
              <el-option
                v-for="item in catalogOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <!-- 标签 -->
          <template>
            <span>标签</span>
            <el-select style="width:18%" v-model="selects.tags" placeholder="请选择">
              <el-option
                v-for="item in tagsOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <!-- 关键字 -->
          <template>
            <span>关键字</span>
            <el-select style="width:18%" v-model="selects.keyword" placeholder="请选择">
              <el-option
                v-for="item in keywordOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
        </div>
        <div class="hmmm_centre">
           <!-- 实体类型 -->
           <template>
            <span>实体类型</span>
            <el-select style="width:18%" v-model="selects.questionType" placeholder="请选择">
              <el-option
                v-for="item in questionTypeOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <!-- 难度 -->
          <template>
            <span>难度</span>
            <el-select style="width:18%" v-model="selects.difficulty" placeholder="请选择">
              <el-option
                v-for="item in difficultyOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <!-- 方向 -->
          <template>
            <span>方向</span>
            <el-select style="width:18%" v-model="selects.direction" placeholder="请选择">
              <el-option
                v-for="item in directionOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <!-- 录入人 -->
          <template>
            <span>录入人</span>
            <el-select style="width:18%" v-model="selects.creatorID" placeholder="请选择">
              <el-option
                v-for="item in creatorOptions"
                :key="item.id"
                :label="item.username"
                :value="item.id">
              </el-option>
            </el-select>
          </template>
        </div>
        <div class="hmmm_buttom">
          <span>题目备注</span>
          <el-input v-model="selects.remarks" style="width:15%"></el-input>
          <span>企业简称</span>
          <el-input v-model="selects.shortName" style="width:15%"></el-input>
          <span>城市</span>
          <el-select @change="secarhChange" style="width:10%" v-model="selects.city" placeholder="请选择">
              <el-option
                v-for="item in cityOptions"
                :key="item"
                :label="item"
                :value="item">
              </el-option>
          </el-select>
          <el-select style="width:10%" v-model="selects.province" placeholder="请选择">
              <el-option
                v-for="item in provinceOptions"
                :key="item"
                :label="item"
                :value="item">
              </el-option>
          </el-select>

          <!-- 按钮 -->
            <el-button @click="delFrom"  style="background-color:#fff; margin-left: 85px;">清除</el-button>
            <el-button @click="SearchFrom" style="background-color:#3d91ea;color: #fff;">搜索</el-button>
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

       <div class="hmmm_tabs">

          <el-tabs v-model="selects.chkState" type="card" @tab-click="handleClick">
            <el-tab-pane  label="全部" name="全部">
            </el-tab-pane>
            <el-tab-pane label="待审核" name="0"></el-tab-pane>
            <el-tab-pane label="已审核" name="1"></el-tab-pane>
            <el-tab-pane label="已拒绝" name="2"></el-tab-pane>
          </el-tabs>

        <el-table
           :data="tableData"
           style="width: 100%">
           <el-table-column
             prop="number"
             label="试题编号"
             width="180">
           </el-table-column>
           <el-table-column
             prop="subject"
             label="学科"
             width="180">
           </el-table-column>
           <el-table-column
             prop="catalog"
             label="目录">
           </el-table-column>
           <el-table-column
             prop="questionType"
             label="题型">
           </el-table-column>
           <el-table-column
             label="题干">
             <div slot-scope="{row}" v-html="row.question"></div>
           </el-table-column>
           <el-table-column
             label="录入时间">
              <template slot-scope="{row}">
                {{ $dayjs(row.addDate).format('YYYY-MM-DD  HH:mm:ss')  }}
              </template>
           </el-table-column>
           <el-table-column
             prop="difficulty"
             label="难度">
           </el-table-column>
           <el-table-column
             prop="creator"
             label="录入人">
           </el-table-column>
           <el-table-column
             prop="chkState"
             label="审核状态">
             <template slot-scope="{row}">
               {{chkStateList[row.chkState]}}
             </template>
           </el-table-column>
           <el-table-column
             prop="chkRemarks"
             label="审核意见">
           </el-table-column>
           <el-table-column
             prop="chkUser"
             label="审核人">
           </el-table-column>
           <el-table-column
             prop="publishState"
             label="发布状态">
             <template slot-scope="{row}">
               {{row.publishState === 1 ? "上架" : '已下架'}}
             </template>
           </el-table-column>
           <el-table-column
             label="操作">
           <template slot-scope="{row}">
              <el-button @click="ISpreview(row)" type="text">预览</el-button>
                <el-button type="text">审核</el-button>
                <el-button type="text">修改</el-button>
                <el-button type="text">上架</el-button>
                <el-button @click="delOption(row)" type="text">删除</el-button>
          </template>
           </el-table-column>
        </el-table>

         <div class="block">
    <!-- @size-change="handleSizeChange"
    @current-change="handleCurrentChange" -->
    <el-pagination
      @size-change="handleSiz"
      @current-change="Currentchange"
      :page-size="selects.pagesize"
      :page-sizes="[5, 10, 20, 50]"
      layout="prev, pager,next,sizes,total, jumper"
      :total="table">
    </el-pagination>
         </div>
    <el-dialog
      title="题目预览"
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
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs'
import { choice, remove } from '../../api/hmmm/questions'
import { simple } from '../../api/hmmm/subjects'
import { simple as simples } from '../../api/base/users'
import { provinces, citys } from '../../api/hmmm/citys'
export default {
  data () {
    return {
      editDialogVisible: false,
      // 城市下拉框
      cityOptions: [],
      // 城市省份下拉框
      provinceOptions: [],
      // 录入人下拉框
      creatorOptions: [],
      // 方向下拉框
      directionOptions: [
        {
          value: '1',
          label: 'o2o'
        },
        {
          value: '2',
          label: '外包服务'
        },
        {
          value: '3',
          label: '企业服务'
        },
        {
          value: '4',
          label: '互联网金融'
        },
        {
          value: '5',
          label: '企业咨询'
        },
        {
          value: '6',
          label: '互联网'
        },
        {
          value: '7',
          label: '电子商务'
        },
        {
          value: '8',
          label: '其他'
        }
      ],
      // 难度下拉框
      difficultyOptions: [
        {
          value: '1',
          label: '简单'
        },
        {
          value: '2',
          label: '一般'
        },
        {
          value: '3',
          label: '困难'
        }
      ],
      // 试题类型下拉框
      questionTypeOptions: [
        {
          value: '1',
          label: '单选'
        },
        {
          value: '2',
          label: '多选'
        },
        {
          value: '3',
          label: '简答'
        }
      ],
      // 关键字下拉框
      keywordOptions: [],
      // 标签下拉框
      tagsOptions: [],
      // 学科下拉框
      subjectOptions: [],
      // 二级目录下拉框
      catalogOptions: [],
      lableTitle: ['全部', '待审核', '已审核', '拒绝'],
      chkStateList: {
        0: '待审核',
        1: '通过',
        2: '拒绝'
      },
      table: 0,
      tableData: [],
      options: [],
      selects: {
        page: 1,
        pagesize: 10,
        chkState: '全部',
        // 学科
        subjectID: '',
        // 二级目录
        catalogID: '',
        // 标签
        tags: '',
        // 关键字
        keyword: '',
        // 实体类型
        questionType: '',
        // 难度
        difficulty: '',
        // 方向
        direction: '',
        // 录入人
        creatorID: '',
        // 题目备注
        remarks: '',
        // 企业简称
        shortName: '',
        // 城市
        city: '',
        // 城市所在省份
        province: ''
      }
    }
  },
  created () {
    this.QuestSimple()
    this.QuestionsList()
    dayjs()
  },
  methods: {
    async QuestionsList () {
      const form = {}
      for (const key in this.selects) {
        if (this.selects[key]) {
          form[key] = this.selects[key]
        }
      }
      if (form.chkState === '全部') {
        delete form.chkState
      }
      const { data } = await choice(form)
      this.tableData = data.items
      this.table = data.counts
      // console.log(data)
    },
    handleClick () {
      // console.log(this.chkState)
      this.QuestionsList()
    },
    handleSiz (val) {
      this.selects.page = 1
      this.selects.pagesize = val
      this.CompanysList()
    },
    Currentchange (val) {
      this.selects.page = val
      this.CompanysList()
    },
    async QuestSimple () {
      const { data } = await simple()
      // console.log(data)
      this.subjectOptions = data
      const res = await simples()
      // console.log(res)
      this.creatorOptions = res.data
      this.cityOptions = provinces()
      citys()
    },
    ISpreview () {
      this.editDialogVisible = true
    },
    secarhChange (val) {
      this.provinceOptions = citys(val)
    },
    delFrom () {
      this.selects = {
        // 学科
        subjectID: '',
        // 二级目录
        catalogID: '',
        // 标签
        tags: '',
        // 关键字
        keyword: '',
        // 实体类型
        questionType: '',
        // 难度
        difficulty: '',
        // 方向
        direction: '',
        // 录入人
        creatorID: '',
        // 题目备注
        remarks: '',
        // 企业简称
        shortName: '',
        // 城市
        city: '',
        // 城市所在省份
        province: ''
      }
    },
    delOption (row) {
      // console.log(row)
      this.$confirm('此操作将永久删除该文件, 是否继续?', '警告', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        await remove({ id: row.id })
        // console.log('qqqqq')
        this.QuestionsList()
        this.$message({
          type: 'success',
          message: '删除成功'
        })
      }).catch(() => {
        this.$message({
          type: 'error',
          message: '取消删除'
        })
      })
    },
    SearchFrom () {
      this.QuestionsList()
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

      .hmmm_folt{
        display: flex;
        justify-content: space-between;
        margin-bottom: 20px;
        .ppppp{
          color: red;
        }
      }
      .hmmm_top{
        span{
          font-weight: 700;
          color: #767175;
          margin-right: 10px;
          margin-left: 10px;
        }
      }

      .hmmm_centre{
        margin-top: 20px;
        span{
          font-weight: 700;
          margin-left: 10px;
          margin-right: 10px;
          color: #767175;
        }
     }

     .hmmm_buttom{
      margin-top: 20px;
      span{
        font-weight: 700;
        margin-left: 10px;
        margin-right: 10px;
        color: #767175;
      }

     }

     .hmmm_tabs{
      margin-top: 20px;
     }
    }
  }
}
</style>
