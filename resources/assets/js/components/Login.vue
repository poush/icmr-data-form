<template>
  <section class="login">
    <header class="login-header">
      <h1 class="brand"><router-link to="/" tabindex="-1">ICMR</router-link></h1>
      <el-alert v-if="error" :title="error.title" type="warning" :description="error.message" show-icon/>
    </header>
    <el-form class="login-form" auto-complete="off" :model="model" :rules="rules" ref="login-form" label-position="top">
      <h2 class="heading">Sign-in</h2>
      <el-form-item label="Login" prop="username">
        <el-input type="text" v-model="model.username" placeholder="Please enter username"/>
      </el-form-item>
      <el-form-item label="Password" prop="password">
        <el-input type="password" v-model="model.password" placeholder="Please enter password"/>
      </el-form-item>
      <el-button type="primary" :loading="loading" @click="submit('login-form')">{{ loading ? 'Loading...' : 'Login' }}</el-button>
    </el-form>
  </section>
</template>

<script>
  export default {
    name: 'login',
    title: 'Login « ICMR',
    data () {
      // form model
      // TODO: remove default values
      const model = {
        username: 'zce',
        password: 'wanglei'
      }
      // form validate rules
      const rules = {
        username: [
          { required: true, message: 'required' },
          { min: 2, max: 16, message: 'required length between 2 to 16' }
        ],
        password: [
          { required: true, message: 'required' },
          { min: 6, max: 16, message: 'length between 6 and 16' }
        ]
      }
      return { model: model, rules: rules, error: null, loading: false }
    },
    methods: {
      submit (ref) {
        // form validate
        this.$refs[ref].validate(valid => {
          if (!valid) return false
          // validated
          this.error = null
          this.loading = true
          // create token from remote
          this.$store.dispatch('createToken', this.model)
            .then(token => {
              this.$router.replace({ path: this.$route.query.redirect || '/' })
              this.loading = false
            })
            .catch(err => {
              console.error(err)
              this.error = { title: '发生错误', message: '出现异常，请稍后再试！' }
              switch (err.response && err.response.status) {
                case 401:
                  this.error.message = '用户名或密码错误！'
                  break
                case 500:
                  this.error.message = '服务器内部异常，请稍后再试！'
                  break
              }
              this.loading = false
            })
        })
      }
    }
  }
</script>

<style lang="scss">
  @import '../../styles/vars';
  .login {
    flex: 1;
    width: 95%;
    max-width: 22rem;
    margin: 0 auto;
    font-size: .875rem;
    &-header {
      margin-bottom: 1rem;
      .brand {
        margin: 4.5rem 0 3.5rem;
        text-align: center;
        letter-spacing: .125rem;
        a {
          margin: 0;
          color: $brand-color;
          font: 300 3rem sans-serif;
          &:hover {
            color: $brand-hover-color;
            text-shadow: 0 0 1rem $brand-hover-color;
          }
        }
      }
    }
    &-form {
      margin-bottom: 2.5rem;
      padding: 1.875rem 1.25rem;
      background: $login-form-background;
      color: $login-form-color;
      .heading {
        margin: 0 0 1rem;
        font-weight: 400;
        font-size: 1.5rem;
      }
      .el-button {
        margin-top: .5rem;
        width: 100%;
      }
    }
    &-footer {
      margin-bottom: 1rem;
      padding: .625rem;
      border: .0625rem solid $brand-color;
      font-size: .75rem;
      text-align: center;
      a {
        color: $brand-color;
      }
    }
  }
</style>