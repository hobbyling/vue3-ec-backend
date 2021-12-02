<template>
  <nav aria-label="Page navigation example">
    <ul class="pagination justify-content-center">
      <li class="page-item" :class="{'disabled': !pages.has_pre}">
        <a class="page-link" href="#" aria-label="Previous" @click.prevent="changePage('pre', current)">
          <span aria-hidden="true">&laquo;</span>
        </a>
      </li>
      <li class="page-item" v-for="page in pages.total_pages" :key="page" :class="{ 'active': page === pages.current_page }">
        <a class="page-link" href="#" @click.prevent="updatePage(page)">{{page}}</a>
      </li>
      <li class="page-item" :class="{'disabled': !pages.has_next}">
        <a class="page-link" href="#" aria-label="Next" @click.prevent="changePage('next', current)">
          <span aria-hidden="true">&raquo;</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script>
// :pages="{ 頁碼資訊 }"
// @emitPages="更新頁面事件"
export default {
  props: ['pages'],
  data () {
    return {
      current: 1
    }
  },
  methods: {
    updatePage (page) {
      this.current = page
      this.$emit('emit-page', page)
    },
    changePage (type, page) {
      if (type === 'pre') {
        if (page > 1) {
          this.$emit('emit-page', page - 1)
          this.current = page - 1
        }
      } else {
        if (page < this.pages.total_pages) {
          this.$emit('emit-page', page + 1)
          this.current = page + 1
        }
      }
    }
  }
}
</script>
