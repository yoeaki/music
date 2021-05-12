<template>
  <div class="login-wrap">
    <div class="ms-login">
      <div>
      </div>
      <div class="ms-title">管理员登录</div>
      <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content">
        <el-form-item prop="username">
          <el-input v-model="param.name" placeholder="username">
            <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            type="password"
            placeholder="password"
            v-model="param.password"
            @keyup.enter.native="submitForm()"
          >
            <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
          </el-input>
        </el-form-item>
        <div class="login-btn">
          <el-button type="primary" @click="submitForm()">登录</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
  import axiso from 'axios'
  export default {
    data: function() {
      return {
        param: {
          name: '',
          password: '',
        },
        rules: {
          username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
          password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
        },
      };
    },
    methods: {
      submitForm() {
        const _this = this;
        this.$refs.login.validate(valid => {
          if (valid) {
            axiso.post('api/surper/admin/login',this.param).then(function (response) {
              const data = response.data;
              if (data.status === 0) {
                _this.$message.success(data.msg)
                console.log(data)
                localStorage.setItem("ms_username",_this.param.username)
                localStorage.setItem("ms_id",data.data.id)
                localStorage.setItem("ms_role",data.data.role)
                _this.$router.push('/adminDashboard')
              } else if (response.data.status === 1) {
                _this.$message.error(data.msg);
                console.log(data.msg);
                return false;
              }
            }).catch(function (error) {
              console.log(error)
            })
          } else {
            this.$message.error("登录失败！")
          }
        });
      },
    },
};
</script>

<style scoped>
  .login-wrap {
    position: relative;
    width: 100%;
    height: 100%;
    background-image: url('../../../assets/img/login-bg.jpg');
    background-size: 100%;
  }
  .ms-title {
    width: 100%;
    line-height: 50px;
    text-align: center;
    font-size: 20px;
    color: #fff;
    border-bottom: 1px solid #ddd;
  }
  .ms-login {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 350px;
    margin: -190px 0 0 -175px;
    border-radius: 5px;
    background: rgba(255, 255, 255, 0.3);
    overflow: hidden;
  }
  .ms-content {
    padding: 30px 30px;
  }
  .login-btn {
    text-align: center;
  }
  .login-btn button {
    width: 100%;
    height: 36px;
    margin-bottom: 10px;
  }
  .login-tips {
    font-size: 12px;
    line-height: 30px;
    color: #fff;
  }
</style>
