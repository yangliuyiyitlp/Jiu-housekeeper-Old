<template>
  <div class="wrap" @keydown="show($event)">
    <h3>赳管家</h3>
    <p v-show="showTip">{{tip}}</p>
    <input type="text" placeholder="请输入用户名" v-model="username">
    <input type="password" placeholder="请输入密码" v-model="password">
    <button @click="login" >登录</button>
  </div>
</template>
<script>
  import Cookie from 'js-cookie'

  export default {
    data () {
      return {
        showTip: false,
        tip: '',
        username: '',
        password: ''
      }
    },
    mounted () {
      /* 页面挂载获取cookie，如果存在username的cookie，则跳转到主页，不需登录 */
      if (Cookie.get('username')) {
        this.$router.push('/home')
      } else {
        this.$router.push('/login')
      }
    },
    methods: {
      show: function (ev) {
        if (ev.keyCode === 13) {
          this.login()
        }
      },
      login () {
        let vm = this
        if (this.username === '' || this.password === '') {
          alert('请输入用户名或密码')
        } else {
          let data = {'username': this.username, 'password': this.password}
          return new Promise((resolve, reject) => {
            this.$ajax({
              url: '/login/submit',
              method: 'post',
              data: data
            }).then(res => {
              /* 接口的传值是(-1,该用户不存在),(0,密码错误)，同时还会检测管理员账号的值 */
              if (res.data.status === 500) {
                this.tip = res.data.msg
                this.showTip = true
//              }
//              else if (res.data.code === 'main') {   // 创建登录页login.vue时，做的判断是当用户名和密码都为admin时，认为它是管理员账号，跳转到管理页main.vue
//                /* 路由跳转this.$router.push */
//                this.$router.push('/main')
              } else if (res.data.status === 200 && res.data.sessionId !== '' && res.data.sessionId !== undefined) {
                Cookie.remove('sessionId')
                Cookie.set('sessionId', res.data.sessionId)
                this.$ajax.get('/login/right', {params: {sessionId: res.data.sessionId}})
                  .then((res) => {
                    if (res.data.code === 500) {
                      vm.tip = res.data.msg
                      this.showTip = true
                    } else {
                      let extendsRoutes = res.data
                      // 存菜单
                      sessionStorage.setItem('menus', JSON.stringify(extendsRoutes))
                      Cookie.remove('username')
                      Cookie.set('username', this.username)
                      // 跳转界面
                      vm.$router.push({path: '/home'})
                    }
                  })
              }
              resolve(res)
            })
              .catch(err => {
                reject(vm.tip = '登录失败' + err)
                this.showTip = true
              })
          })
        }
      }
    }
  }
</script>

<style scoped>
  html, body {
    width: 100%;
    text-align: center;
  }

  .wrap {
    text-align: center;
    margin: 200px auto;
    position: relative;
    width: 300px;
    padding: 25px 29px 29px;
    background-color: #fff;
    border: 1px solid #e5e5e5;
  }

  h3 {
    font-family: Helvetica, Georgia, Arial, sans-serif, 黑体;
    font-size: 36px;
    margin-bottom: 20px;
    color: #0663a2;
  }

  input {
    display: block;
    width: 250px;
    height: 40px;
    line-height: 40px;
    margin: 0 auto;
    margin-bottom: 10px;
    outline: none;
    border: 1px solid #888;
    padding: 10px;
    box-sizing: border-box;
  }

  p {
    color: red;
  }

  button {
    display: block;
    width: 250px;
    height: 40px;
    line-height: 40px;
    margin: 0 auto;
    border: none;
    background-color: #20a0ff;
    color: #fff;
    font-size: 16px;
    margin-bottom: 5px;
    cursor: pointer;
  }

  span {
    cursor: pointer;
  }

  span:hover {
    color: #41b883;
  }
</style>
