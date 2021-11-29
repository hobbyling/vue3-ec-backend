<template>
  <div class="text-end">
    <button
      class="btn btn-primary"
      type="button"
      @click="openModal(true)"
    >
      新增
    </button>
  </div>
  <table class="table mt-4">
    <thead>
      <tr>
        <th width="120">分類</th>
        <th>產品名稱</th>
        <th width="120">原價</th>
        <th width="120">售價</th>
        <th width="100">是否啟用</th>
        <th width="200">編輯</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="item in products"
        :key="item.id"
      >
        <td>{{ item.category }}</td>
        <td>{{ item.title }}</td>
        <td class="text-right">
          {{ item.origin_price }}
        </td>
        <td class="text-right">
          {{ item.price }}
        </td>
        <td>
          <span v-if="item.is_enabled" class="text-success">啟用</span>
          <span v-else class="text-muted">未啟用</span>
        </td>
        <td>
          <div class="btn-group">
            <button
              class="btn btn-outline-primary btn-sm"
              @click="openModal(false, item)"
            >
              編輯
            </button>
            <button
              class="btn btn-outline-danger btn-sm"
              @click="openDeleteModal(item)"
            >
              刪除
            </button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
  <ProductModal ref="productModal" :product="tempProduct" @update-product="updateProducts" />
  <DeleteModal ref="deleteModal" :product="tempProduct" @delete-product="deleteProduct" />
</template>

<script>
import ProductModal from '../components/ProductModal.vue'
import DeleteModal from '../components/DeleteModal.vue'
export default {
  name: 'Products',
  components: { ProductModal, DeleteModal },
  data () {
    return {
      products: [],
      pagination: {},
      tempProduct: {},
      isNew: false
    }
  },
  created () {
    this.getProducts()
  },
  methods: {
    // 開啟新增/編輯燈箱
    openModal (isNew, item) {
      this.tempProduct = isNew
        ? {}
        : { ...item }

      this.isNew = isNew
      const productComponent = this.$refs.productModal
      productComponent.showModal()
    },
    // 開啟刪除燈箱
    openDeleteModal (item) {
      this.tempProduct = { ...item }
      const productComponent = this.$refs.deleteModal
      productComponent.showModal()
    },
    // 取得產品列表
    getProducts () {
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/products`
      this.$http.get(api)
        .then((res) => {
          if (res.data.success) {
            this.products = res.data.products
            this.pagination = res.data.pagination
          }
        })
    },
    // 更新產品
    updateProducts (item) {
      const productComponent = this.$refs.productModal

      // 若是新增狀態
      let api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/product`
      let httpMethod = 'post'

      // 若是編輯狀態
      if (!this.isNew) {
        api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/product/${item.id}`
        httpMethod = 'put'
      }
      // 打 API
      this.$http[httpMethod](api, { data: item })
        .then((res) => {
          if (res.data.success) {
            productComponent.hideModal()
            this.getProducts()
          }
        })
    },
    // 刪除產品
    deleteProduct (id) {
      console.log(id)
      const productComponent = this.$refs.deleteModal
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/product/${id}`
      // 打 API
      this.$http.delete(api)
        .then((res) => {
          if (res.data.success) {
            productComponent.hideModal()
            this.getProducts()
          }
        })
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
