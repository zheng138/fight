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
            难度:
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
          <el-col :span="6" placeholder="请选择">4444</el-col>
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
        difficulty: '',
        questionType: ''
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
.wd {
  width: 170px;
}
</style>
