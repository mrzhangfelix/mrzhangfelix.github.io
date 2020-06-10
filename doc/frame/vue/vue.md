
## axios请求工具类
```javascript
import axios from 'axios'
import {Message} from 'element-ui'

//全局拦截
axios.interceptors.request.use(config => {
    config.baseURL = '/api'
    config.withCredentials = true // 允许携带token ,这个是解决跨域产生的相关问题
    // config.timeout = 6000
    let token = sessionStorage.getItem('token')
    if (token) {
      config.headers = {
        'access-token': token,
        'Content-Type': 'application/x-www-form-urlencoded'
      }
    }
    return config;
  },
  err => {
    Message.error({message: '请求超时!'});
    // return Promise.resolve(err);
  })
axios.interceptors.response.use(
  data => {
    // 内部服务器错误
    if (data.status && data.status == 500) {
      Message.error({message: '内部服务器错误' + data.data.message});
      return;
    }
    if (data.data.msg) {
      Message.success({message: data.data.message});
    }
    return data;
  },
  err => {
    if ((typeof err.response) == 'undefined') {
      Message.error({message: '未找到返回信息，请重新登陆'});
      return new Error('未登录');
    } else if (err.response.status == 504 || err.response.status == 404) {
      Message.error({message: '网关超时或服务未找到'});
    } else if (err.response.status == 403) {
      Message.error({message: '权限不足,请联系管理员!'});
    } else if (err.response.status == 401) {
      Message.error({message: '401:未授权' + err.response.data.message});
    } else {
      if (err.response.data.msg) {
        Message.error({message: err.response.data.message});
      } else {
        Message.error({message: '未知错误!'});
      }
    }
    // return Promise.resolve(err);
  })
let base = '';
export const postRequest = (url, params) => {
  return axios({
    method: 'post',
    url: `${base}${url}`,
    data: params,
    transformRequest: [function (data) {
      let ret = ''
      for (let it in data) {
        ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
      }
      return ret
    }],
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
    }
  });
}
export const uploadFileRequest = (url, params) => {
  return axios({
    method: 'post',
    url: `${base}${url}`,
    data: params,
    headers: {
      'Content-Type': 'multipart/form-data',
    }
  });
}
export const putRequest = (url, params) => {
  return axios({
    method: 'put',
    url: `${base}${url}`,
    data: params,
    transformRequest: [function (data) {
      let ret = ''
      for (let it in data) {
        ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
      }
      return ret
    }],
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
    }
  });
}
export const deleteRequest = (url) => {
  return axios({
    method: 'delete',
    url: `${base}${url}`,
    headers: {
    }
  });
}
export const getRequest = (url) => {
  return axios({
    method: 'get',
    url: `${base}${url}`,
    headers: {
    }
  });
}

```


## 登录页面
```vue
<template>
    <div>
        <el-form
                :rules="rules"
                ref="loginForm"
                v-loading="loading"
                element-loading-text="正在登录..."
                element-loading-spinner="el-icon-loading"
                element-loading-background="rgba(0, 0, 0, 0.8)"
                :model="loginForm"
                class="loginContainer">
            <h3 class="loginTitle">系统登录</h3>
            <el-form-item prop="username">
                <el-input size="normal" type="text" v-model="loginForm.username" auto-complete="off"
                          placeholder="请输入用户名"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input size="normal" type="password" v-model="loginForm.password" auto-complete="off"
                          placeholder="请输入密码"></el-input>
            </el-form-item>
            <el-button size="normal" type="primary" style="width: 100%;" @click="submitLogin">登录</el-button>
        </el-form>
    </div>
</template>

<script>

    export default {
        name: "Login",
        data() {
            return {
                loading: false,
                loginForm: {
                    username: 'admin',
                    password: ''
                },
                rules: {
                    username: [{required: true, message: '请输入用户名', trigger: 'blur'}],
                    password: [{required: true, message: '请输入密码', trigger: 'blur'}]
                }
            }
        },
        methods: {
            submitLogin() {
                this.$refs.loginForm.validate((valid) => {
                    if (valid) {
                        this.loading = true;
                        this.postRequest('/login/userLogin', this.loginForm).then(resp => {
                            this.loading = false;
                            if (resp) {
                                let token = resp.data.token;
                                console.log(token);
                                sessionStorage.clear();
                                window.sessionStorage.setItem("token", token);
                                let path = this.$route.query.redirect;
                                this.$router.replace((path == '/' || path == undefined) ? '/index' : path);
                            }
                        })
                    } else {
                        return false;
                    }
                });
            }
        }
    }
</script>

<style>
    .loginContainer {
        border-radius: 15px;
        background-clip: padding-box;
        margin: 180px auto;
        width: 350px;
        padding: 15px 35px 15px 35px;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #cac6c6;
    }

    .loginTitle {
        margin: 15px auto 20px auto;
        text-align: center;
        color: #505458;
    }
</style>

```