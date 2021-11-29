<template>
  <div
    ref="modal"
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-xl" role="document">
      <div class="modal-content border-0">
        <div class="modal-header bg-dark text-white">
          <h5 class="modal-title" id="exampleModalLabel">
            <span>刪除{{tempProduct.title}}</span>
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close">
          </button>
        </div>
        <div class="modal-body">
          是否刪除 <span class="text-danger">{{tempProduct.title}}</span>（刪除後將無法恢復）。
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-outline-secondary"
            data-bs-dismiss="modal"
          >
            取消
          </button>
          <button
            type="button"
            class="btn btn-primary"
            @click="$emit('delete-product', tempProduct.id)"
          >
            確認刪除
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import modalMixin from '@/mixins/modalMixin'
export default {
  name: 'ProductModal',
  props: {
    product: {
      type: Object,
      default () {
        return {}
      }
    }
  },
  mixins: [modalMixin],
  data () {
    return {
      modal: {},
      tempProduct: {}
    }
  },
  watch: {
    product (newVal) {
      this.tempProduct = this.product
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
