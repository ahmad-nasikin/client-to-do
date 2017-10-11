<template>
  <div class="container">
    <div class="text-header">
    <h3>Todo</h3>
    </div>
    <div class="col-md-6 col-md-offset-3">
      <form class="form-horizontal">
        <div class="form-group">
          <div class="col-lg-10">
            <input v-model="login.username" type="text" class="form-control" placeholder="Username" required>
          </div>
        </div>
        <div class="form-group">
          <div class="col-lg-10">
            <input v-model="login.password" type="password" class="form-control" placeholder="Password" required>
          </div>
        </div>
        <div class="form-group text-right">
          <div class="col-lg-10 col-lg-offset-2">
            <div class="form-group">
              <button type="button" class="btn btn-primary" @click="Login()" data-dismiss="modal">Login</button>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      login: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    Login () {
      var self = this
      this.$http.post('/login', {
        username: self.login.username,
        password: self.login.password
      })
      .then(response => {
        console.log('respone', response)
        localStorage.setItem('token', response.data.token)
        this.$router.push('/todo')
      })
      .catch(err => {
        console.error(err)
      })
    }
  }
}
</script>

<style lang="css">
</style>
