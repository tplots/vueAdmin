<template>
    <div class="login-container">
        <el-form ref="loginForm" :model="loginForm" :rules="loginRules" auto-complete="on" label-position="left" label-width="0px" class="card-box login-form">
            <h3 class="title">vue-element-admin</h3>
            <el-form-item prop="username">
                <span class="svg-container svg-container_login">
                    <svg-icon icon-class="user" />
                </span>
                <el-input v-model="loginForm.username" name="username" type="text" auto-complete="on" placeholder="username" />
            </el-form-item>
            <el-form-item prop="password">
                <span class="svg-container">
                    <svg-icon icon-class="password" />
                </span>
                <el-input v-model="loginForm.password" :type="pwdType" name="password" auto-complete="on" placeholder="password" @keyup.enter.native="handleLogin" />
                <span class="show-pwd" @click="showPwd">
                    <svg-icon icon-class="eye" />
                </span>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" style="width:100%;" @click.native.prevent="handleLogin">
                    Sign in
                </el-button>
            </el-form-item>
            <div class="tips">
                <span style="margin-right:20px;">username: admin</span>
                <span> password: admin</span>
            </div>
        </el-form>
    </div>
</template>

<script>
import { isvalidUsername } from '@/utils/validate';

export default {
    name: 'Login',
    data() {
        const validateUsername = (rule, value, callback) => {
            if (!isvalidUsername(value)) {
                callback(new Error('该用户不存在'));
            } else {
                callback();
            }
        };
        const validatePass = (rule, value, callback) => {
            if (value.length < 3) {
                callback(new Error('密码不能小于5位'));
            } else {
                callback();
            }
        };
        return {
            loginForm: {
                username: 'GH001',
                password: 'H001'
            },
            loginRules: {
                username: [{ required: true, trigger: 'blur', validator: validateUsername }],
                password: [{ required: true, trigger: 'blur', validator: validatePass }]
            },
            pwdType: 'password'
        };
    },
    methods: {
        showPwd() {
            if (this.pwdType === 'password') {
                this.pwdType = '';
            } else {
                this.pwdType = 'password';
            }
        },
        handleLogin() {
            this.$refs.loginForm.validate(valid => {
                if (valid) {
                    this.$store
                        .dispatch('Login', this.loginForm)
                        .then(() => {
                            this.$router.push({ path: '/' });
                        })
                        .catch(err => {
                            this.$message.error(err.message);
                        });
                } else {
                    return false;
                }
            });
        }
    }
};
</script>

<style rel="stylesheet/scss" lang="scss">
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;

.login-container {
    position: fixed;
    height: 100%;
    width: 100%;
    background-color: $bg;
    input:-webkit-autofill {
        box-shadow: 0 0 0px 1000px #293444 inset !important;
        -webkit-text-fill-color: #fff !important;
    }
    input {
        background: transparent;
        border: 0px;
        -webkit-appearance: none;
        border-radius: 0px;
        padding: 12px 5px 12px 15px;
        color: $light_gray;
        height: 47px;
    }
    .el-input {
        display: inline-block;
        height: 47px;
        width: 85%;
    }
    .tips {
        font-size: 14px;
        color: #fff;
        margin-bottom: 10px;
    }
    .svg-container {
        padding: 6px 5px 6px 15px;
        color: $dark_gray;
        vertical-align: middle;
        width: 30px;
        display: inline-block;
        &_login {
            font-size: 20px;
        }
    }
    .title {
        font-size: 26px;
        font-weight: 400;
        color: $light_gray;
        margin: 0px auto 40px auto;
        text-align: center;
        font-weight: bold;
    }
    .login-form {
        position: absolute;
        left: 0;
        right: 0;
        width: 400px;
        padding: 35px 35px 15px 35px;
        margin: 120px auto;
    }
    .el-form-item {
        border: 1px solid rgba(255, 255, 255, 0.1);
        background: rgba(0, 0, 0, 0.1);
        border-radius: 5px;
        color: #454545;
    }
    .show-pwd {
        position: absolute;
        right: 10px;
        top: 7px;
        font-size: 16px;
        color: $dark_gray;
        cursor: pointer;
        user-select: none;
    }
    .thirdparty-button {
        position: absolute;
        right: 35px;
        bottom: 28px;
    }
}
</style>
