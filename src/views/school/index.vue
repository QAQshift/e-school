<template>
  <div class="app-container">
    <XHeader title="学校" />

    <div class="app-upload">
      <span>创建学校：</span>
      <el-input v-model="schoolName" style="margin-right:10px;flex:1;" />
      <el-button type="primary" @click="onSubmit">创建</el-button>
    </div>
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >

      <el-table-column align="center" label="ID" width="120">
        <template slot-scope="scope">
          {{ scope.row.cmpId }}
        </template>
      </el-table-column>
      <el-table-column align="center" label="校区">
        <template slot-scope="scope">
          {{ scope.row.cmpName }}
        </template>
      </el-table-column>
      <el-table-column align="center" label="创建时间" width="500">
        <template slot-scope="scope">
          {{ scope.row.cmpCreateTime }}
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="100" class-name="small-padding fixed-width">
        <template slot-scope="{row,$index}">
          <el-button size="mini" type="danger" @click="handleDelete(row,$index)">
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import XHeader from '@/components/Header'
// import { getList } from '@/api/test/schoolT'
import { uploadCampus, deleteCampus, getCampus } from '@/api/campus'
export default {
  name: 'SwiperUpload',
  components: { XHeader },
  filters: {

  },
  data() {
    return {
      list: null,
      listLoading: true,
      schoolName: ''

    }
  },
  watch: {

  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      getCampus().then(response => {
        console.log('response', response)
        this.list = response.data.data
        this.listLoading = false
      })
    },
    onSubmit() {
      uploadCampus({ cmpName: this.schoolName }).then(res => {
        this.$message({
          message: '添加成功',
          type: 'success'
        })
        this.fetchData()
        this.schoolName = ''
      })
    },
    handleDelete(row, index) {
      deleteCampus({ cmpId: row.cmpId }).then(res => {
        this.$message({
          message: '删除成功',
          type: 'success'
        })
        this.fetchData()
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.app {
  &-container {
    width:100%;
    box-sizing: border-box;
    padding:10px;
  }
  &-upload{
    margin-bottom:20px;
    display: flex;
    align-items:center;
    span{
      font-size:15px;
      color:#000;
      font-weight:900;

    }
  }
}
</style>
