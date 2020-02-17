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
            <el-select class="wd" v-model="searchForm.tags" placeholder="请选择"></el-select>
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
            <el-select class="wd" v-model="searchForm.direction" placeholder="请选择"></el-select>
          </el-col>
          <!-- --------------------------------------录入人--------------------------------------------------------- -->
          <el-col :span="6">
            录入人:
            <el-select class="wd" v-model="searchForm.creatorID" placeholder="请选择"></el-select>
          </el-col>
          <!-- --------------------------------------二级目录--------------------------------------------------------- -->
          <el-col :span="6">
            二级目录:
            <el-select class="wd" v-model="searchForm.catalogID" placeholder="请选择"></el-select>
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
import { simple } from '@/api/hmmm/subjects.js'
// 2导入难度的api
import {
  difficulty as difficultyList,
  questionType as questionTypeList
} from '@/api/hmmm/constants.js'

export default {
  name: 'QuestionsList',
  data() {
    return {
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
    this.getSubjectIDList()
  },
  methods: {
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
