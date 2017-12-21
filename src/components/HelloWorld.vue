<template>
  <div>
    <input type="text" key="username" v-model="username">
    <input type="password" key="password" v-model="password">
    <button v-on:click="login">登录</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      username: '',
      password: ''
    }
  },
  methods: {
    login: function () {
      // let vm = this
      let params = {
        username: this.username,
        password: this.password
      }
      this.$http.post('api/login', params).then(function (res) {
        console.log(res)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
