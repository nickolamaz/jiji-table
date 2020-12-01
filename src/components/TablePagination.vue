<template>
  <div>
    <nav aria-label="...">
      <ul class="pagination">
        <li :class="`page-item ${currentPage === page ? 'active' : ''}`"
            v-for="(page, idx) in totalPages"
            @click="goToPage(page)"
            :key="idx">
          <span class="page-link">
            {{ page }}
          </span>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  name: 'TablePagination',
  props: {
    pageLimit: {
      type: Number,
      required: true,
    },
    pageSize: {
      type: Number,
      required: true,
    },
    disabled: {
      type: Boolean,
      required: true,
    }
  },
  data() {
    return {
      currentPage: null,
      totalPages: null,
    }
  },
  watch: {
    pageLimit() {
      this.getPaginationPages();
    }
  },
  created() {
    this.getPaginationPages();
    this.$nextTick(() => this.goToPage(this.totalPages[0]));
  },
  methods: {
    getPaginationPages() {
      const totalPages = Math.trunc((this.pageLimit + this.pageSize - 1) / this.pageSize);
      this.totalPages = Array(totalPages)
          .fill(null)
          .map((_, idx) => (idx + 1));
    },
    goToPage(num) {
      if (this.disabled) {
        return;
      }
      this.currentPage = num;
      this.$emit('goToPage', num);
    }
  }
}
</script>

<style>
.pagination {
  display: flex;
  padding-left: 0;
  list-style: none;
  border-radius: .25rem;
}

.page-link {
  position: relative;
  display: block;
  padding: .5rem .75rem;
  margin-left: -1px;
  line-height: 1.25;
  color: #007bff;
  background-color: #fff;
  border: 1px solid #dee2e6;
  cursor: pointer;
}

.page-item:first-child .page-link {
  margin-left: 0;
  border-top-left-radius: .25rem;
  border-bottom-left-radius: .25rem;
}
.page-item:last-child .page-link {
  border-top-right-radius: .25rem;
  border-bottom-right-radius: .25rem;
}

.page-item.active .page-link {
  z-index: 3;
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}

</style>
