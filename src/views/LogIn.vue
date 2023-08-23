<template>
  <!-- <h1>This is login page</h1> -->
  <div class="container mt-5">
    <form
      class="row d-flex justify-content-center align-items-center"
      @submit.prevent="signIn">
      <div class="col-md-6">
        <h1 class="h3 mb-4 font-weight-bold text-center">請先登入</h1>
        <div class="mb-3">
          <label for="inputEmail" class="sr-only">Email</label>
          <input
            type="email"
            id="inputEmail"
            class="form-control"
            placeholder="Please enter email address..."
            required
            autofocus
            v-model="user.username"
            />
        </div>
        <div class="mb-3">
          <label for="inputPassword" class="sr-only">Password</label>
          <input
            type="password"
            id="inputPassword"
            class="form-control"
            placeholder="Please enter password..."
            required
            autofocus
            v-model="user.password"
            />
        </div>
        <div class="text-end mt-4">
          <button type="submit" class="btn btn-primary btn-block">登入</button>
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
