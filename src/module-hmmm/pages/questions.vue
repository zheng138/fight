<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card class="box-card">
        <el-row :gutter="20">
          <el-col :span="6">
            学科:
            <el-select class="wd" v-model="searchForm.subjectID" placeholder="请选择">
              <el-option
                v-for="item in subjectIDList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            难&nbsp;&nbsp;&nbsp;度:
            <el-select class="wd" v-model="searchForm.difficulty" placeholder="请选择">
              <!-- 3对数组做遍历 -->
              <el-option
                v-for="item in  difficultyList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            试题类型:
            <el-select class="wd" v-model="searchForm.questionType" placeholder="请选择">
              <el-option
                v-for="item in  questionTypeList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <!-- --------------------------------------标签--------------------------------------------------------- -->
          <el-col :span="6">
            标&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;签:
            <el-select class="wd" v-model="searchForm.tags" placeholder="请选择">
              <el-option
                v-for="item in   tagsList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
        </el-row>
        <!-- --------------------------------------城市--------------------------------------------------------- -->
        <el-row :gutter="20">
          <el-col :span="6">
            城市:
            <el-select
              style="width:85px"
              v-model="searchForm.province"
              placeholder="选城市"
              @change="getCitys(searchForm.province)"
            >
              <!-- v-for='item in data成员/methods成员' -->
              <!-- methods成员在模板中也可以使用，注意设置( )即可
              {{provin()}}-->
              <el-option v-for="item in provin()" :key="item" :value="item" :label="item"></el-option>
            </el-select>
            <el-select style="width:85px" v-model="searchForm.city" placeholder="选地区">
              <el-option v-for="item in  cityList" :key="item" :value="item" :label="item"></el-option>
            </el-select>
          </el-col>
          <!-- -------------------------------------- 关键字--------------------------------------------------------- -->
          <el-col :span="6">
            关键字:
            <el-input v-model="searchForm.keyword" class="wd"></el-input>
          </el-col>
          <!-- --------------------------------------题目备注--------------------------------------------------------- -->
          <el-col :span="6">
            题目备注:
            <el-input v-model="searchForm.remarks" class="wd"></el-input>
          </el-col>
          <!-- -------------------------------------- 企业简称--------------------------------------------------------- -->
          <el-col :span="6">
            企业简称:
            <el-select class="wd" v-model="searchForm.shortName" placeholder="请选择"></el-select>
          </el-col>
        </el-row>
        <!-- --------------------------------------方向--------------------------------------------------------- -->
        <el-row :gutter="20">
          <el-col :span="6">
            方向:
            <el-select class="wd" v-model="searchForm.direction" placeholder="请选择">
              <!-- <el-option
                v-for="item in directionList"                         
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>-->
              <!-- 方向和其他的不一样 -->
              <el-option v-for="(item,k) in directionList" :key="k" :value="item" :label="item"></el-option>
            </el-select>
          </el-col>
          <!-- --------------------------------------录入人--------------------------------------------------------- -->
          <el-col :span="6">
            录入人:
            <el-select class="wd" v-model="searchForm.creatorID" placeholder="请选择">
              <el-option
                v-for="item in   creatorIDList"
                :key="item.id"
                :value="item.id"
                :label="item.username"
              ></el-option>
            </el-select>
          </el-col>
          <!-- --------------------------------------二级目录--------------------------------------------------------- -->
          <el-col :span="6">
            二级目录:
            <el-select class="wd" v-model="searchForm.catalogID" placeholder="请选择">
              <el-option
                v-for="item in  catalogIDList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <!-- -----------------------------------------结束--------------------------------------------------------------- -->
          <el-col :span="6">
            <el-button size="mini">清除</el-button>
            <el-button type="primary">搜索</el-button>
          </el-col>
        </el-row>
        <!-- 给table加了border就是给小表格加了边线了 -->
        <el-table border :data="questionList" style="width:100%">
          <el-table-column label="序号" type="index" width="60"></el-table-column>
          <el-table-column label="试题编号" prop="number"></el-table-column>
          <el-table-column label="学科" prop="subject"></el-table-column>
          <!-- :formatter="questionTypeFMT",对列内容进行二次更新，注意不要设置()括号 -->
          <!-- 有了此属性，当前列的内容就会受到questionTypeFMT的返回值决定了 -->
          <el-table-column :formatter="questionTypeFMT" label="题型" prop="questionType"></el-table-column>
          <el-table-column label="题干" prop="question"></el-table-column>
          <el-table-column label="录入时间" width="170">
            <!-- 通过插值表达式表现时间信息，并应用过滤器做格式转换 -->
            <!-- 获得当前列的信息，作用域插槽应用 -->
            <span slot-scope="stDate">{{stDate.row.addDate |parseTimeByString}}</span>
          </el-table-column>
          <el-table-column :formatter="difficultyTypeFMT" label="难度" prop="difficulty"></el-table-column>
          <el-table-column label="录入人" prop="creator"></el-table-column>
          <el-table-column label="操作" width="200">
            <a href="#">预览</a>
            <a href="#">修改</a>
            <a href="#">删除</a>
            <a href="#">加入精选</a>
          </el-table-column>
        </el-table>
      </el-card>
    </div>
  </div>
</template>

<script>
// 导入基础题库列表展示的api
import { list } from '@/api/hmmm/questions.js'
// 导入城市 区县的api
import { provinces, citys } from '@/api/hmmm/citys.js'
// 输入二级目录的api
import { simple as directorysSimple } from '@/api/hmmm/directorys.js'
// 导入录入人的api
import { simple as usersSimple } from '@/api/base/users.js'
// 导入标签的api 因为有两个一模一样的simple，需要起一个别名
import { simple as tagsSimple } from '@/api/hmmm/tags.js'
// 2导入学科列表的api
import { simple } from '@/api/hmmm/subjects.js'
// 2导入难度的api
import {
  difficulty as difficultyList,
  questionType as questionTypeList,
  // 2 导入方向的api
  direction as directionList
} from '@/api/hmmm/constants.js'

export default {
  name: 'QuestionsList',
  data() {
    return {
      // 试题列表
      questionList: [],
      // 获得区县的下拉列表i
      cityList: [],
      // 方向下拉列表
      directionList,
      // 录入二级目录下拉列表
      catalogIDList: [],
      // 录入人下拉列表
      creatorIDList: [],
      // 1 标签数据，用于给下拉列表展示
      tagsList: [],
      // 学科列表数据，用于给下拉列表展示使用
      subjectIDList: [],
      // 1在data中声明数组，因为我们要遍历这个数组
      // 4因为difficultyList和difficulty没有任何关系，我们
      // 要将difficultyList和difficulty联系起来
      difficultyList,
      questionTypeList,
      searchForm: {
        page: 1, // 默认页码
        pagesize: 10, // 默认数据条数
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 试题类型
        tags: '', // 标签
        province: '', // 城市
        city: '', // 地区
        keyword: '', // 关键字
        remarks: '', // 题目标注
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '' // 目录
      }
    }
  },
  watch: {
    // 搜索表单深度监听
    searchForm: {
      handler: function(newV, oldV) {
        // 任何条件变化，都重新获得数据
        this.getQuestionList()
      },
      deep: true
    }
  },
  created() {
    // 学科
    this.getSubjectIDList()
    // 标签
    this.getTagsList()
    // 录入人
    this.getCreatorIDList()
    // 录入二级目录
    this.getCatalogIDList()
    // // 城市
    // this.provin()
    // 基础题库列表
    this.getQuestionList()
  },
  methods: {
    // 难度数字转汉字
    difficultyTypeFMT(row, column, cellValue, index) {
      // console.log(row)
      // return row
      // return row.answer
      // return column
      // console.log(column)
      // return column.id
      // return cellValue
      // return row.questionType
      // return this.questionTypeList[cellValue - 1].label

      // 把cellValue的汉字内容进行显示
      // return this.questionTypeList[cellValue - 1].label
      return this.difficultyList[row.difficulty - 1].label
    },
    // 对题型 列 信息 进行二次更新操作
    // row:代表当前每个数据项目记录信息的，是一个对象，{id:xxx,number:xx,difficult:xx,addDate:xx....}
    //     可以通过这个row访问到当前任何数据项目记录信息，调用形式：row.id row.number
    // column：可以获取记录的列的信息，一般不使用
    // cellValue当前行当前列的数据项目记录信息，类似row.questionType
    questionTypeFMT(row, column, cellValue, index) {
      // console.log(row)
      // return row
      // return row.answer
      // return column
      // console.log(column)
      // return column.id
      // return cellValue
      // return row.questionType
      // return this.questionTypeList[cellValue - 1].label

      // 把cellValue的汉字内容进行显示
      // return this.questionTypeList[cellValue - 1].label
      return this.questionTypeList[row.questionType - 1].label
    },

    // 获得基础列表
    async getQuestionList() {
      // 调用api,并传递参数,设置数据查询条件
      let result = await list(this.searchForm)
      this.questionList = result.data.items
    },
    // 获得 城市 信息
    // 这个prame形参就代表被选中的省份信息
    getCitys(pname) {
      this.searchForm.city = '' // 清除之前选取好的城市
      this.cityList = citys(pname)
    },
    // 接收城市的函数
    provin: provinces,
    // 导入二级目录下拉列表的方法
    async getCatalogIDList() {
      let result = await directorysSimple()
      this.catalogIDList = result.data
      // console.log(result)
    },
    // 导入录入人下拉列表的方法
    async getCreatorIDList() {
      let result = await usersSimple()
      // 用data接收信息
      this.creatorIDList = result.data
      //  console.log(result)
    },
    // 导入标签下拉列表数据的方法
    async getTagsList() {
      // 调用api获得数据
      let result = await tagsSimple()
      // data接收
      this.tagsList = result.data
    },

    // 3导入学科的方法
    async getSubjectIDList() {
      let result = await simple()
      // data接受信息
      this.subjectIDList = result.data
      // console.log(result)
    }
  }
}
</script>

<style scoped>
.el-table {
  margin-top: 25px;
}
.el-row {
  margin-bottom: 10px;
}
.wd {
  width: 170px;
}
</style>
