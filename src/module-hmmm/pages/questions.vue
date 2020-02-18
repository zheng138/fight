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
            <el-select style="width:85px" v-model="searchForm.province" placeholder="选城市"></el-select>
            <el-select style="width:85px" v-model="searchForm.citys" placeholder="选地区"></el-select>
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
      </el-card>
    </div>
  </div>
</template>

<script>
// 导入城市 区县的api
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
      // // 城市下拉列表
      // provinceList: [],
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
  created() {
    // 学科
    this.getSubjectIDList()
    // 标签
    this.getTagsList()
    // 录入人
    this.getCreatorIDList()
    // 录入二级目录
    this.getCatalogIDList()
  },
  methods: {
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
.el-row {
  margin-bottom: 10px;
}
.wd {
  width: 170px;
}
</style>
