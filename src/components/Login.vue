<template>
  <div class="container wrap  wrap-tablet wrap-mobile full">
    <div class="flex basis12">
        <h1><p>NodeTube</p></h1>
    </div>
    <div class="flex basis12 full">
        <div class="flex2">
            <img src="./../assets/logo.png">
        </div>
        <div class="flex2">
            <button class="btn" @click="login()">Login</button>
        </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'login',
    data () {
      return {
      }
    },
    methods: {
      login: function () {
        this.$http.get('/youtube/auth')
          .then(
            response => {
              console.log(response.body)
              var windowOpen = window.open(response.body, 'google', 'width=300px, height:300px')
              var polltime = setInterval(() => {
                try {
                  if (windowOpen.document.URL.indexOf('http://localhost:8080') !== -1) {
                    windowOpen.close()
                    window.clearInterval(polltime)
                    var urlCode = windowOpen.document.URL
                    var idx = urlCode.lastIndexOf('code=')
                    var code = urlCode.substring(idx + 5).replace(/#/g, '')
                    console.log(code)
                    this.getToken(code)
                  }
                } catch (err) {
                  console.log(err)
                }
              })
            })
          .catch(
            err => {
              console.log(err)
            })
      },
      getToken: function (code) {
        this.$http.get(`/youtube/token?code=${code}`)
          .then(
            response => {
              window.location.href += 'musics'
            })
          .catch(
            err => {
              alert('YOU SHAL NOT CONNECT!')
              console.log(err)
            })
      }
    }
  }
</script>

<style>

</style>