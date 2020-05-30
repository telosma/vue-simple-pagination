<script>
import { PAGINATION } from '../constants'

const chunk = (input, size) => {
  return input.reduce((arr, item, idx) => {
    return idx % size === 0
      ? [...arr, [item]]
      : [...arr.slice(0, -1), [...arr.slice(-1)[0], item]]
  }, [])
}

export default {
  props: {
    currentPage: {
      type: Number,
      default: 1,
      required: true
    },
    totalCount: {
      type: Number,
      default: 0,
      required: true
    },
    perPage: {
      type: Number,
      default: 1,
      required: true
    },
    limit: {
      type: Number,
      default: PAGINATION.limit
    },
    type: {
      type: String,
      default: PAGINATION.type.default
    }
  },

  data: () => ({
    pagination: PAGINATION
  }),

  computed: {
    totalPage () {
      return Math.floor(this.totalCount / this.perPage) +
        (this.totalCount % this.perPage > 0 ? 1 : 0)
    },

    isShowPrev () {
      return this.currentPage > 1
    },

    isShowNext () {
      return this.totalPage > 1 && this.currentPage < this.totalPage
    },

    // @return Number value of last page previous chunk
    firstPageOfPrevChunk () {
      const currentChunk = Math.floor((this.currentPage - 1) / this.limit)
      let prevChunkPage = 0
      if (currentChunk > 0) {
        const prevChunk = currentChunk - 1
        prevChunkPage = (prevChunk * this.limit) + this.limit
      }

      return prevChunkPage
    },

    // @return Number value of first page number next chunk
    firstPageOfNextChunk () {
      const currentChunk = Math.floor((this.currentPage - 1) / this.limit)
      const nextChunk = currentChunk + 1
      const nextChunkPage = (nextChunk * this.limit) + 1
      if (nextChunkPage <= this.totalPage) {
        return nextChunkPage
      } else {
        return 0
      }
    },

     // @return Array number of page value
    pages () {
      let pageLists = []
      const arrayRangePage = [...Array(this.totalPage).keys()].map(v => v+1)
      if (this.totalPage <= this.limit || this.limit === 0) {
        pageLists = arrayRangePage
      } else {
        const chunkPages = chunk(arrayRangePage, this.limit)
        const currentChunk = Math.floor((this.currentPage - 1) / this.limit)
        pageLists = chunkPages[currentChunk]
      }

      return pageLists
    }
  },

  methods: {
    // @eventHandler
    handleClickPage (page) {
      this.$emit('change', page)
    },

    // @eventHandler
    handleClickPrev () {
      if (!this.isShowPrev) {
        return
      }

      const prev = this.currentPage - 1
      prev > 0 ? this.$emit('change', prev) : this.$emit('change', 1)
    },

    // @eventHandler
    handleClickNext () {
      if (!this.isShowNext) {
        return
      }

      const next = this.currentPage + 1
      next <= this.totalPage ? this.$emit('change', next) : this.$emit('change', this.totalPage)
    },

    /**
     * @ventHandler
     * @description: Trigger custom event `change` on parent component
     * @param: value of next page
    **/
    handleClickDoubleNext (next) {
      if (!next) { return }
      this.$emit('change', next)
    },

    /**
     * @ventHandler
     * @description: Trigger custom event `change` on parent component
     * @param: value of previous page
     **/
    handleClickDoublePrev (prev) {
      if (!prev) { return }
      this.$emit('change', prev)
    }
  }
}
</script>
<template>
<div v-if="totalPage > 0" class="pagination-container row">
  <div v-if="type === pagination.type.default" class="p-default ui tiny menu pagination" id="menu-pagination">
    <a
      :class="[ !!firstPageOfPrevChunk ? '' : 'disabled', 'item' ]"
      href="#"
      @click.prevent="handleClickDoublePrev(firstPageOfPrevChunk)">
      <i class="angle double left icon"></i>
    </a>
    <a
      :class="[ isShowPrev ? '' : 'disabled', 'item' ]"
      href="#"
      @click.prevent="handleClickPrev">
      <i class="angle left icon"></i>
    </a>
    <a
      v-for="(page, index) in pages"
      :key="index"
      :class="{ 'active': page == currentPage }"
      class="item"
      href="#"
      @click.prevent="handleClickPage(page, $event)"
    >{{page}}</a>
    <a
      :class="[ isShowNext ? '' : 'disabled', 'item' ]"
      href="#"
      @click.prevent="handleClickNext">
      <i class="angle right icon"></i>
    </a>
    <a
      :class="[ !!firstPageOfNextChunk ? '' : 'disabled', 'item' ]"
      href="#"
      @click.prevent="handleClickDoubleNext(firstPageOfNextChunk)">
      <i class="angle double right icon"></i>
    </a>
  </div>
  <div v-if="type === pagination.type.simple" class="p-simple column">
    <div class="arrow-container ui centered grid">
      <span
        :class="[
          'arrow-left ui aligned left floated',
          isShowPrev ? 'clickable' : 'disabled'
        ]"
        @click.prevent="handleClickPrev"
      ></span>
      <span class="ui middle center aligned">{{ currentPage }} / {{ totalPage }}</span>
      <span
        :class="[
          'arrow-right ui aligned right floated',
          isShowNext ? 'clickable' : 'disabled'
        ]"
        @click.prevent="handleClickNext"
      ></span>
    </div>
  </div>
</div>
</template>
<style lang="scss" scoped>
#menu-pagination {
  margin-top: 3rem;
  margin-bottom: 1rem;
}
.pagination-container {
  margin-bottom: 2rem;
  .p-simple {
    .arrow-container {
      .arrow-left {
        &.clickable {
          cursor: pointer;
          border-right: 23px solid #3f8bbf;
        }
        display: inline-block;
        border-top: 14px solid transparent;
        border-bottom: 14px solid transparent;
        border-right:23px solid #98C1DD;
      }
      .arrow-right {
        margin-right: 8px;
        display: inline-block;
        border-top: 14px solid transparent;
        border-bottom: 14px solid transparent;
        border-left: 23px solid #98C1DD;
      }
      .arrow-right {
        &.clickable {
          cursor: pointer;
          border-left: 23px solid #3f8bbf;
        }
      }
    }
  }
}
</style>
