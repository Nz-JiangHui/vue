<template>
  <div class="ui right floated pagination menu">
    <a class="icon item" @click="first()">
      First
    </a>
    <a class="icon item" @click="prev()">
      <i class="left chevron icon"></i>
    </a>
    <a class="item more" v-if="showPrevMore">
      ...
    </a>
    <a class="item" v-for="page in pagers" :class="{active: index == page}" @click="go(page)">
      {{page}}
    </a>
    <a class="item more" v-if="showNextMore">
      ...
    </a>
    <a class="icon item" @click="next()">
      <i class="right chevron icon"></i>
    </a>
    <a class="icon item" @click="last()">
      End
    </a>
  </div>
</template>

<script>
  export default {
    name: 'pagination',
    props: {
      perPage: {
        type: Number,
        default: 5
      },
      pageIndex: {
        type: Number,
        default: 1
      },
      pageSize: {
        type: Number,
        default: 10
      },
      total: {
        type: Number,
        default: 1
      }
    },
    data () {
      return {
        index: this.pageIndex,
        limit: this.pageSize,
        size: this.total || 1,
        showPrevMore: false,
        showNextMore: false
      }
    },
    methods: {
      go (page) {
        if (this.index !== page) {
          this.index = page
          this.$emit('change', this.index)
        }
      },
      prev () {
        if (this.index > 1) {
          this.go(this.index - 1)
        }
      },
      next () {
        if (this.index < this.pages) {
          this.go(this.index + 1)
        }
      },
      first () {
        if (this.index !== 1) {
          this.go(1)
        }
      },
      last () {
        if (this.index != this.pages) {
          this.go(this.pages)
        }
      }
    },
    computed: {
      pages () {
        return Math.ceil(this.size / this.limit)
      },
      pagers () {
        const arr = []
        const perPages = this.perPage
        const pageCount = this.pages
        let current = this.index
        const _offset = (perPages - 1) / 2
        const offset = {
          start: current - _offset,
          end: current + _offset
        }
        if (offset.start < 1) {
          offset.end = offset.end + (1 - offset.start)
          offset.start = 1
        }
        if (offset.end > pageCount) {
          offset.start = offset.start - (offset.end - pageCount)
          offset.end = pageCount
        }
        if (offset.start < 1) {
          offset.start = 1
        }
        this.showPrevMore = (offset.start > 1)
        this.showNextMore = (offset.end < pageCount)

        for (let i = offset.start; i <= offset.end; i++) {
          arr.push(i)
        }
        return arr
      }
    },
    watch: {
      pageIndex (val) {
        this.index = val || 1
      },
      pageSize (val) {
        this.limit = val || 10
      },
      total (val) {
        this.size = val || 1
      }
    }
  }
</script>

<style scoped>
  .more:hover {
    cursor: default !important;
    background-color: transparent !important;
  }
</style>
