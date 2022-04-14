<template>
  <div class="app-container">
    <!-- 输入表单 -->
    <el-form label-width="120px">
      <el-form-item label="借款额度">
        <el-input-number v-model="integralGrade.borrowAmount" :min="0" />
      </el-form-item>
      <el-form-item label="积分区间开始">
        <el-input-number v-model="integralGrade.integralStart" :min="0" />
      </el-form-item>
      <el-form-item label="积分区间结束">
        <el-input-number v-model="integralGrade.integralEnd" :min="0" />
      </el-form-item>
      <el-form-item>
        <el-button
          :disabled="saveBtnDisabled"
          type="primary"
          @click="saveOrUpdate()"
        >
          保存
        </el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
// 引入api模块
import integralGrade from '@/api/core/integral-grade';
export default {
  data() {
    return {
      saveBtnDisabled: false, //是否禁用保存按钮,防止表单重复提交
      integralGrade: {}, // 积分等级对象
    };
  },
  created() {
    if (this.$route.params.id) {
      this.fetchById(this.$route.params.id);
    }
  },
  methods: {
    fetchById(id) {
      integralGrade.getById(id).then((responce) => {
        this.integralGrade = responce.data.record;
      });
    },
    saveOrUpdate() {
      // 禁用保存按钮
      this.saveBtnDisabled = true;
      if (this.integralGrade.id) {
        // 更新
        this.updateData();
      } else {
        // 新增
        this.saveData();
      }
    },
    saveData() {
      integralGrade.save(this.integralGrade).then((responce) => {
        this.$message.success(responce.message);
        this.$router.push('/core/integral-grade/list');
      });
    },
    updateData() {
      integralGrade.updateById(this.integralGrade).then((responce) => {
        this.$message.success(responce.message);
        this.$router.push('/core/integral-grade/list');
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
