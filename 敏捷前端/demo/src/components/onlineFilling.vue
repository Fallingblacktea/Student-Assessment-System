<template>
  <div>
    <el-card>
      <h3 class="card-title">学生信息填写</h3>
      <el-form :model="form" label-position="top">
        <el-form-item label="1. 个人学年总结">
          <el-input
            type="textarea"
            v-model="form.personalSummary"
            placeholder="请输入个人学年总结，字数不超过400字"
            :rows="5"
            :autosize="{ minRows: 5, maxRows: 5 }"
          ></el-input>
        </el-form-item>

        <el-form-item label="2. 成绩(GPA)">
          <el-input-number v-model="form.GPA" :step="0.01" :precision="2" placeholder="请输入gpa" ></el-input-number>
        </el-form-item>

        <el-form-item label="3. 科研情况">
          <el-table :data="form.researchStatus" style="width: 100%">
            <el-table-column prop="ranking" label="名次">
              <template #default="scope">
                <el-input v-model="scope.row.ranking"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="time" label="时间">
              <template #default="scope">
                <el-input v-model="scope.row.time" placeholder="格式为YYYY-MM"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="location" label="地点">
              <template #default="scope">
                <el-input v-model="scope.row.location"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="award" label="奖项">
              <template #default="scope">
                <el-input v-model="scope.row.award"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="level" label="级别">
              <template #default="scope">
                <el-input v-model="scope.row.level"></el-input>
              </template>
            </el-table-column>
            <el-table-column>
              <template #default="scope">
                <el-button type="text" @click="removeResearchItem(scope.$index)">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
          <el-button @click="addResearchItem">添加</el-button>
        </el-form-item>

        <el-form-item label="4. 学生骨干服务岗位">
          <el-table :data="form.studentService" style="width: 100%">
            <el-table-column prop="startTime" label="起始时间">
              <template #default="scope">
                <el-input v-model="scope.row.startTime" placeholder="格式为YYYY-MM"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="positionName" label="服务岗位名称">
              <template #default="scope">
                <el-input v-model="scope.row.positionName"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="positionTitle" label="职务">
              <template #default="scope">
                <el-input v-model="scope.row.positionTitle"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="assessmentLevel" label="考核级别">
              <template #default="scope">
                <el-input v-model="scope.row.assessmentLevel"></el-input>
              </template>
            </el-table-column>
            <el-table-column>
              <template #default="scope">
                <el-button type="text" @click="removeServiceItem(scope.$index)">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
          <el-button @click="addServiceItem">添加</el-button>
        </el-form-item>

        <el-form-item label="5. 社会实践情况">
          <el-table :data="form.socialPractice" style="width: 100%">
            <el-table-column prop="initiatingOrganization" label="发起单位">
              <template #default="scope">
                <el-input v-model="scope.row.initiatingOrganization"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="location" label="地点">
              <template #default="scope">
                <el-input v-model="scope.row.location"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="duration" label="天数">
              <template #default="scope">
                <el-input-number v-model="scope.row.duration" :step="1" :precision="0"></el-input-number>
              </template>
            </el-table-column>
            <el-table-column prop="teamSize" label="团队人数">
              <template #default="scope">
                <el-input-number v-model="scope.row.teamSize" :step="1" :precision="0"></el-input-number>
              </template>
            </el-table-column>
            <el-table-column prop="teamPosition" label="团内职务">
              <template #default="scope">
                <el-input v-model="scope.row.teamPosition"></el-input>
              </template>
            </el-table-column>
            <el-table-column prop="isOnline" label="是否线上">
              <template #default="scope">
                <el-checkbox v-model="scope.row.isOnline"></el-checkbox>
              </template>
            </el-table-column>
            <el-table-column prop="hasAward" label="是否获得奖励">
              <template #default="scope">
                <el-checkbox v-model="scope.row.hasAward"></el-checkbox>
              </template>
            </el-table-column>
            <el-table-column prop="awardName" label="奖励名称">
              <template #default="scope">
                <el-input v-model="scope.row.awardName"></el-input>
              </template>
            </el-table-column>
            <el-table-column>
              <template #default="scope">
                <el-button type="text" @click="removePracticeItem(scope.$index)">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
          <el-button @click="addPracticeItem">添加</el-button>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm">提交以上（1-5项）信息</el-button>
          请勿忘记提交证明材料！
        </el-form-item>
        <div class="form-item-wrapper">
            <el-form-item label="6. 证明材料上传(请上传jpeg和png格式的文件)">
              <input type="file" @change="handleFileUpload">
    <button @click="submitFile">提交</button>
          </el-form-item>
          </div>
      </el-form>
    </el-card>
  </div>
</template>

<script>
import { ElMessage } from 'element-plus';
import { submitfile,onlineFillingSubmit } from '/src/api/submit.js'
export default {
  data() {
    return {
      file:null,
      form: {
        personalSummary: "",
        GPA: null,
        researchStatus: [],
        studentService: [],
        socialPractice: []
      }
    };
  },

  methods: {
    handleFileUpload(event) {
      this.file = event.target.files[0];
    },
    async submitFile() {
      if (this.file) {
        const allowedTypes = ['image/jpeg', 'image/png']; // 允许的文件类型
        const maxSize = 2 * 1024 * 1024; // 最大文件大小为 2MB
        console.log(this.file.type);
    if (!allowedTypes.includes(this.file.type)) {
      ElMessage.error('只能上传 JPEG/PNG 格式的图片');
      return; // 如果文件类型不正确，中断上传操作
    }
    if (this.file.size > maxSize) {
      ElMessage.error('上传文件大小不能超过2MB');
      return; // 如果文件大小超过限制，中断上传操作
    }
        const formData = new FormData();
        formData.append('file', this.file); // 将文件添加到FormData对象中，'file'是服务器端接收文件的字段名
        try {
          const response = await submitfile(formData); // 使用submit函数发送FormData对象
          ElMessage.success('上传成功'); // 处理上传成功后的逻辑
        } catch (error) {
          ElMessage.error('上传失败'); // 处理上传失败的逻辑
        }
        // 可以使用 FormData 或其他方式将文件发送到服务器
        console.log("提交文件:", this.file);
      }
    },
    addResearchItem() {
      this.form.researchStatus.push({});
    },
    removeResearchItem(index) {
      this.form.researchStatus.splice(index, 1);
    },
    addServiceItem() {
      this.form.studentService.push({});
    },
    removeServiceItem(index) {
      this.form.studentService.splice(index, 1);
    },
    addPracticeItem() {
      this.form.socialPractice.push({});
    },
    removePracticeItem(index) {
      this.form.socialPractice.splice(index, 1);
    },
    submitForm() {
      // 处理表单提交逻辑
      // 执行表单提交
      onlineFillingSubmit(this.form)
        .then(response => {
          ElMessage.success('提交成功');
          // 其他成功处理逻辑
        })
        .catch(error => {
          ElMessage.error('提交失败');
          // 其他失败处理逻辑
        });
    }
  }
};
</script>

<style scoped>
.card-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}
.form-item-wrapper {
    margin-top: 20px; /* 这里可以调整平移的距离 */
  }
</style>
