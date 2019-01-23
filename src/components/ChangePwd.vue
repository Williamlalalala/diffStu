<template>
    <el-row>
      <el-col :span="10" :offset="6">
          <el-form label-width="100px" class="demo-ruleForm" :model="passwordForm" status-icon :rules="passwordRules" ref="passwordForm">
            <el-form-item label="旧密码" prop="oldPassword">
                <el-input type="password" v-model="passwordForm.oldPassword" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="新密码" prop="newPassword">
                <el-input type="password" v-model="passwordForm.newPassword" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="确认密码" prop="checkPass">
                <el-input type="password" v-model="passwordForm.checkPass" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('passwordForm')">提交</el-button>
                <el-button @click="resetForm('passwordForm')">重置</el-button>
            </el-form-item>
          </el-form>
      </el-col>
    </el-row>
</template>
<script>
import router from '../router'
export default {
    data() {
      var validateOldPass = (rule,value,callback) => {
          if(value === ''){
              callback(new Error('请输入旧密码'));
          } else {
              if(this.passwordForm.newPassword !== ''){
                  this.$refs.passwordForm.validateField('newPassword');
              }
              callback();
          }
      };
      var validateNewPass = (rule,value,callback) => {
          if(value === '') {
              callback(new Error('请输入新密码'));
          } else if(value == this.passwordForm.oldPassword){
              callback(new Error('新密码不能和旧密码一样'))
          } else {
              if(this.passwordForm.checkPass !== ''){
                  this.$refs.passwordForm.validateField('checkPass');
              }
              callback();
          }
      };
      var validateCheckPass = (rule,value,callback) => {
          if(value === ''){
              callback(new Error('请再次输入密码'));
          }else if(value !== this.passwordForm.newPassword) {
              callback(new Error('两次输入密码不一致！'));
          }else{
              callback();
          }
      };
      return {
          passwordForm:{
              oldPassword:'',
              newPassword:'',
              checkPass:''
          },
          passwordRules:{
              oldPassword:[{validator:validateOldPass,trigger:'blur'}],
              newPassword:[{validator:validateNewPass,trigger:'blur'}],
              checkPass:[{validator:validateCheckPass,trigger:'blur'}]
          }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
            if (valid) {
                var data = JSON.stringify({
                  "teacherId":sessionStorage.getItem('userName'),
                  "oldPassword":this.passwordForm.oldPassword,
                  "newPassword":this.passwordForm.newPassword
              })
              var self = this
              $.ajax({
                  url:"http://"+self.Global.ipAddr+"/newhelp/api/teacher",
                  type:"PUT",
                  contentType: "application/json; charset=utf-8",
                  data:data,
                  dataType:"json",
                  beforeSend:function(request){
                      request.setRequestHeader("Authorization",sessionStorage.getItem('token'));
                    },
                  success:function(data){
                      if(data.success){
                          alert(data.message);
                          self.$router.push('/')
                        }else{
                            alert(data.message);
                        }
                    },
                error:function(data){
                    alert("密码修改失败"+data.status);
                },
              })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  }
</script>
<style scoped>
</style>
