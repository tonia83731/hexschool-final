<template>
  <!-- <h1>This is products page</h1> -->
  <LoadingOverlay :active="isLoading"></LoadingOverlay>
  <div class="text-end">
    <button class="btn btn-vintage-l-green" type="button" @click="openModal(true)">增加一個產品</button>
  </div>
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
        <td class="text-right">{{ $filters.currency(item.origin_price) }}</td>
        <td class="text-right">{{ $filters.currency(item.price) }}</td>
        <td class="text-success" v-if="item.is_enabled">啟用</td>
        <td class="text-muted" v-else>未啟用</td>
        <td>
          <button type="button" class="btn btn-border-vintage-tiffany btn-sm" @click="openModal(false, item)">編輯</button>
          <button type="button" class="btn btn-border-vintage-red btn-sm ms-2">刪除</button>
        </td>
      </tr>
    </tbody>
  </table>
  <Pagination :pages="pagination" @emit-page="getProducts"></Pagination>
  <ProductModal ref="productModal" :product="tempProduct" @update-product="updateProduct"></ProductModal>
</template>
<script>
import ProductModal from '@/components/ProductModal.vue'
import Pagination from '@/components/PaginationView.vue'
// import { currency } from '../methods/filters'

export default {
  data () {
    return {
      products: [],
      pagination: {},
      tempProduct: {},
      isNew: false,
      isLoading: false
    }
  },
  inject: ['emitter'],
  components: {
    ProductModal,
    Pagination
  },
  methods: {
    // currency,
    getProducts (page = 1) {
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/products?page=${page}`
      // ?page=:page
      this.isLoading = true
      this.$http.get(api)
        .then((res) => {
          console.log(res)
          this.isLoading = false
          if (res.data.success) {
            // console.log(res.data)
            this.products = res.data.products
            this.pagination = res.data.pagination
          }
        })
    },
    openModal (isNew, item) {
      // console.log(isNew, item)
      if (isNew) {
        this.tempProduct = {}
      } else {
        this.tempProduct = { ...item }
      }
      this.isNew = isNew
      const productComponent = this.$refs.productModal
      productComponent.showModal()
    },
    updateProduct (item) {
      // console.log(item)
      this.tempProduct = item
      // 新增狀態
      let api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/product`
      let httpMethod = 'post'
      // 編輯狀態
      if (!this.isNew) {
        api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/product/${item.id}`
        httpMethod = 'put'
      }
      const productComponent = this.$refs.productModal
      this.$http[httpMethod](api, { data: this.tempProduct }).then((res) => {
        // console.log(res)
        productComponent.hideModal()
        if (res.data.success) {
          this.getProducts()
          this.emitter.emit('push-message', {
            style: 'success',
            title: '更新成功'
          })
        } else {
          this.emitter.emit('push-message', {
            style: 'danger',
            title: '更新失敗',
            content: res.data.message.join('、')
          })
        }
      })
    }
  },
  created () {
    this.getProducts()
  }
}
</script>
