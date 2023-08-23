<template>
  <!-- <h1>This is products page</h1> -->
  <table class="table mt-4">
    <thead>
      <tr>
        <th width="120">分類</th>
        <th width="120">產品名稱</th>
        <th width="120">原價</th>
        <th width="120">售價</th>
        <th width="100">是否啟用</th>
        <th width="200">編輯</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in products" :key="item.id">
        <td>{{ item.category }}</td>
        <td>{{ item.title }}</td>
        <td class="text-right">{{ item.origin_price }}</td>
        <td class="text-right">{{ item.price }}</td>
        <td class="text-success" v-if="item.is_enabled">啟用</td>
        <td class="text-muted" v-else>未啟用</td>
        <td>
          <button type="button" class="btn btn-outline-primary btn-sm">編輯</button>
          <button type="button" class="btn btn-outline-danger btn-sm ms-2">刪除</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  data () {
    return {
      products: [],
      pagination: {}
    }
  },
  methods: {
    getProducts () {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/products`
      // ?page=:page
      this.$http.get(api)
        .then((res) => {
          // console.log(res)
          if (res.data.success) {
            console.log(res.data)
            this.products = res.data.products
            this.pagination = res.data.pagination
          }
        })
    }
  },
  created () {
    this.getProducts()
  }
}
</script>
