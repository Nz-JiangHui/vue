<template>
  <div>
    <input type="text" key="username" v-model="username">
    <input type="password" key="password" v-model="password">
    <button v-on:click="login()">登录</button>
    <pagination
      :page-index="pageConf.currentPage"
      :total="pageConf.totalNum"
      :page-size="pageConf.perPage"
      @change="pageChange"
    ></pagination>
  </div>
</template>

<script>
  import pagination from './pagination'

  export default {
    components: {
      pagination
    },
    name: 'HelloWorld',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        username: '',
        password: '',
        pageConf: {
          perPage: 10,
          currentPage: 1,
          totalNum: 1
        }
      }
    },
    methods: {
      login: function () {
        let params = {
          username: this.username,
          password: this.password
        }
        this.$http.post('api/login', params).then((res) => {
          if (res.data.code === 200) {
            this.$router.push({name: 'home'})
          }
        })
      },
      pageChange (page) {
        this.pageConf.currentPage = page
        //
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
