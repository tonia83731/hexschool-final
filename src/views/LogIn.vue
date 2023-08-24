<template>
  <!-- <h1>This is login page</h1> -->
  <div class="container">
    <form
      class="row d-flex justify-content-center align-items-center"
      @submit.prevent="signIn">
      <div class="col-md-6">
        <h1 class="h3 mb-4 text-center vintage-d-green fw-bold">後台登入</h1>
        <div class="form-floating mb-3">
          <input
            type="email"
            class="form-control"
            id="inputEmail"
            placeholder="Please enter email address..."
            required
            autofocus
            v-model="user.username">
          <label for="inputEmail">Email address</label>
        </div>
        <div class="form-floating">
          <input
            type="password"
            id="inputPassword"
            class="form-control"
            placeholder="Please enter password..."
            required
            autofocus
            v-model="user.password">
          <label for="inputPassword">Password</label>
        </div>
        <div class="text-end mt-4">
          <button type="submit" class="btn btn-vintage-l-green btn-block">登入</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    signIn () {
      // console.log('login')
      const api = `${process.env.VUE_APP_API}/admin/signin`
      // console.log(api)
      this.$http.post(api, this.user)
        .then((res) => {
          // console.log(res)
          if (res.data.success) {
            const { token, expired } = res.data
            // console.log(token, expired)
            document.cookie = `hexToken=${token}; expires=${new Date(expired)}`
            this.$router.push('/dashboard/products')
          }
        })
    }
  }
}
</script>
