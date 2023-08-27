<template>
  <!-- <h1>This is DashBoard</h1> -->
  <Navbar></Navbar>
  <div class="container mt-5">
    <router-view/>
  </div>
  <div class="container text-end mt-5">
    <a class="nav-link" href="#" @click.prevent="logOut">
      <i class="fa-solid fa-right-from-bracket"></i>
      登出
    </a>
  </div>

</template>

<script>
import emitter from '@/methods/emitter'
import ToastMessages from '@/components/ToastMessages.vue'
import Navbar from '../components/NavBar.vue'
export default {
  components: {
    // eslint-disable-next-line vue/no-unused-components
    Navbar, ToastMessages
  },
  provide () {
    return {
      emitter
    }
  },
  methods: {
    logOut () {
      const api = `${process.env.VUE_APP_API}/logout`
      // console.log(api)
      this.$http.post(api, this.user)
        .then((res) => {
          // console.log(res)
          if (res.data.success) {
            this.$router.push('/login')
          }
        })
    }
  },
  created () {
    const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1')
    // console.log(token)
    this.$http.defaults.headers.common.Authorization = `${token}`
    const api = `${process.env.VUE_APP_API}/api/user/check`
    // console.log(api)
    this.$http.post(api, this.user)
      .then((res) => {
        // console.log(res)
        if (!res.data.success) {
          this.$router.push('/login')
        }
      })
  }
}
</script>
