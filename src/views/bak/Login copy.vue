<template>
<body id="poster">
    <el-form class="login-container" label-position="left" label-width="0px">
        <h3 class="login_title">系统登录</h3>
        <el-form-item>
            <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="账号"></el-input>
        </el-form-item>
        <el-form-item>
            <el-input type="password" v-model="loginForm.password" auto-complete="off" placeholder="密码" show-password></el-input>
        </el-form-item>

        <el-form :inline="true">
            <el-form-item>
                <el-input class="login-captcha" type="text" v-model="loginForm.captcha" auto-complete="off" placeholder="验证码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-image :src="src" class="login-captcha-img"></el-image>
            </el-form-item>
        </el-form>

        <el-form-item style="width: 100%">
            <el-button type="primary" style="width: 100%;background: #505458;border: none" v-on:click="login">登录</el-button>
        </el-form-item>
    </el-form>
</body>
</template>

<script>
    export default {
        name: 'Login',
        data() {
            return {
                src: 'https://cube.elemecdn.com/6/94/4d3ea53c084bad6931a56d5158a48jpeg.jpeg',
                loginForm: {
                    username: '',
                    password: '',
                    captcha: '',
                },
                responseResult: []
            }
        },
        methods: {
            login() {
                this.$axios
                    .post('/login', {
                        username: this.loginForm.username,
                        password: this.loginForm.password,
                        captcha: this.loginForm.captcha,
                    })
                    .then(successResponse => {
                        if (successResponse.data.code === 200) {
                            this.$router.replace({
                                path: '/index'
                            })
                        }
                    })
                    .catch(failResponse => {})
            }
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
