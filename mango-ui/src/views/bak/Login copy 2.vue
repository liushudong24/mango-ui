<template>
<body id="poster">
    <el-form :model="loginForm" :rules="fieldRules" ref="loginForm" class="login-container" label-position="left" label-width="0px">
        <h3 class="login_title">系统登录</h3>
        <el-form-item prop="account">
            <el-input type="text" v-model="loginForm.account" auto-complete="off" placeholder="账号"></el-input>
        </el-form-item>
        <el-form-item  prop="password">
            <el-input type="password" v-model="loginForm.password" auto-complete="off" placeholder="密码" show-password></el-input>
        </el-form-item>

        <el-form-item >
            <el-col :span="12">
                <el-form-item prop="captcha">
                <el-input type="test" v-model="loginForm.captcha" auto-complete="off" placeholder="验证码, 单击图片刷新"
                    style="width: 100%;">
                </el-input>
                </el-form-item>
            </el-col>
            <el-col class="line" :span="1">&nbsp;</el-col>
            <el-col :span="11">
                <el-form-item>
                    <img style="width: 100%;" class="pointer" :src="loginForm.src" @click="refreshCaptcha">
                </el-form-item>
            </el-col>
        </el-form-item>

        <el-form-item style="width: 100%">
            <el-button tyoe="primary" style="width: 48%;" @click.native.prevent="reset">重 置</el-button>
            <el-button type="primary" style="width: 48%;" @click.native.prevent="login" :loading="loading">登 录</el-button>
        </el-form-item>
    </el-form>
</body>
</template>

<script>
    import Cookies from "js-cookie"
    export default {
        name: 'Login',
        data() {
            return {
                loading: false,
                loginForm: {
                    account: 'admin',
                    password: 'admin',
                    captcha: '',
                    src: '',
                },
                fieldRules: {
                    account: [
                        { required: true, message: '请输入账号', trigger: 'blur'}
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur'}
                    ]
                },
                checked: true
            }
        },
        methods: {
            login() {
                this.loading = true
                let userInfo = {
                    account:this.loginForm.account,
                    password:this.loginForm.password,
                    captcha:this.loginForm.captcha
                }
                console.log(userInfo)
                alert(JSON.stringify(userInfo))
                this.$api.login.login(userInfo).then(
                    (res) => {  // 调用登录接口
                        if(res.msg != null){
                            this.$message({ message: res.msg, type: 'error'})
                        } else {
                            Cookies.set('token', res.data.token) // 设置token到Cookie
                            sessionStorage.setItem('user', userInfo.account) //保存用户到本地会话
                            this.$router.push('/') // 登陆成功，跳转到主页
                        }
                        this.loading = false
                    }).captcha((res) => {
                        this.$message({ message: res.message, type: 'error'})
                })
            },
            refreshCaptcha: function(){
                this.loginForm.src = this.global.baseUrl + "/captcha.jpg?t=" + new Date().getTime();
            },
            reset() {
                this.$refs.loginForm.resetFields()
            }
        },
        mounted() {
            this.refreshCaptcha()
        }
    }
</script>

<style>
    #poster {
        background: url("../assets/eva.jpg") no-repeat;
        background-position: center;
        height: 100%;
        width: 100%;
        background-size: cover;
        position: fixed;
    }

    body {
        margin: 0px;
    }

    .login-container {
        border-radius: 15px;
        background-clip: padding-box;
        margin: 90px auto;
        width: 350px;
        padding: 35px 35px 15px 35px;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #cac6c6;
    }

    .login_title {
        margin: 0px auto 40px auto;
        text-align: center;
        color: #505458;
    }

    .login-captcha {
        width: 200px;
    }

    .login-captcha-img {
        width: 125px;
        height: 40px;
    }
</style>

<!--<template>

<div class="page">
    <h2>Login Page</h2>
    <el-button type="primary" @click="login()">登录</el-button>
  </div>

<div id="app">

<h2>登录系统</h2>

<el-input placeholder="请输入用户姓名" v-model="input1" clearable></el-input>
    <el-input placeholder="请输入密码" v-model="input2" show-password></el-input>
    <el-button type="primary" @click="login()">登录</el-button>

<el-container>

<el-container>

<el-main>Main</el-main>

</el-container>

</el-container>

</div>
</template>

<script src="//unpkg.com/vue/dist/vue.js"></script>
<script src="//unpkg.com/element-ui@2.14.0/lib/index.js"></script><script>
import mock from '@/mock/index.js'
import Cookies from 'js-cookie'
import router from '@/router'
export default {
    name: 'Login',
    methods: {
        login() {
            this.$api.login
                .login()
                .then(function (res) {
                    alert(res.data.token)
                    Cookies.set('token', res.token) // 放置token到Cookie
                    router.push('/') // 登录成功，跳转到主页
                })
                .catch(function (res) {
                    alert(res)
                })
        },
    },
}
</script><script>
<!--
export default {
    data() {
        return {
            input1: '',
            input2: ''
        }
    }
}
</script>-->

<!--<style>
@import url('//unpkg.com/element-ui@2.14.0/lib/theme-chalk/index.css');
.el-header,
.el-footer {

background-color: #b3c0d1;

color: #333;

text-align: center;

line-height: 60px;
}

.el-aside {

background-color: #d3dce6;

color: #333;

text-align: center;

line-height: 200px;
}

.el-main {

background-color: #e9eef3;

color: #333;

text-align: center;

line-height: 160px;
}

body > .el-container {

margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {

line-height: 260px;
}

.el-container:nth-child(7) .el-aside {

line-height: 320px;
}
</style>-->
