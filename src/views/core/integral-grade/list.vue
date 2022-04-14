<template>
  <div class="app-container">
    <el-table :data="list" border style="width: 100%">
      <el-table-column type="index" width="50" />
      <el-table-column prop="borrowAmount" label="借款额度" width="180" />
      <el-table-column prop="integralStart" label="积分区间开始" />
      <el-table-column prop="integralEnd" label="积分区间结束" />
      <el-table-column label="操作">
        <template slot-scope="scope">
          <router-link
            :to="'/core/integral-grade/edit/' + scope.row.id"
            style="margin-right: 15px"
          >
            <el-button type="primary" size="mini" icon="el-icon-edit">
              修改
            </el-button>
          </router-link>
          <el-button
            size="mini"
            type="danger"
            icon="el-icon-delete"
            @click="removeById(scope.row.id)"
          >
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
// 引入api模块
import integralGrade from '@/api/core/integral-grade';
export default {
  data() {
    return {
      list: [], //积分等级列表
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      integralGrade.list().then((responce) => {
        this.list = responce.data.list;
      });
    },
    removeById(id) {
      console.log('id =====> ', id);
      this.$confirm('此操作将永久删除该记录, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
      })
        .then(() => {
          return integralGrade.removeById(id);
        })
        .then((responce) => {
          this.$message({
            message: responce.message,
            type: 'success',
          });
          this.fetchData();
        })
        .catch((error) => {
          if (error === 'cancel') {
            this.$message({
              type: 'info',
              message: '已取消删除',
            });
          }
        });
    },
  },
};
</script>

<style scoped>
.app-container {
  margin: 10px;
}
</style>
