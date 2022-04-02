<template>
  <nav aria-label="Page navigation example">
    <ul class="pagination">
      <li class="page-item">
        <a
          class="page-link"
          href="#"
          aria-label="First"
          @click="onClickFirstPage"
          :disabled="isInFirstPage"
        >
          <span aria-hidden="true">First</span>
        </a>
      </li>
      <li class="page-item">
        <a
          class="page-link"
          href="#"
          aria-label="Previous"
          @click="onClickPreviousPage"
          :disabled="isInFirstPage"
        >
          <span aria-hidden="true">Prev</span>
        </a>
      </li>
      <li
        v-for="page in pages"
        :key="page"
        class="page-item"
        :class="{ active: isPageActive(page.name) }"
      >
        <a
          class="page-link"
          href="#"
          type="button"
          @click="onClickPage(page.name)"
          :disabled="page.isDisabled"
          :aria-label="`Go to page number ${page.name}`"
        >
          {{ page.name }}
        </a>
      </li>
      <li class="page-item">
        <a
          class="page-link"
          href="#"
          aria-label="Next"
          @click="onClickNextPage"
          :disabled="isInLastPage"
        >
          <span aria-hidden="true">Next</span>
        </a>
      </li>
      <li class="page-item">
        <a
          class="page-link"
          href="#"
          aria-label="Last"
          @click="onClickLastPage"
          :disabled="isInLastPage"
        >
          <span aria-hidden="true">Last</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: "PaginationComponent",
  props: {
    maxVisibleButtons: {
      type: Number,
      required: false,
      default: 3,
    },
    totalPages: {
      type: Number,
      required: true,
    },
    total: {
      type: Number,
      required: true,
    },
    perPage: {
      type: Number,
      required: true,
    },
    currentPage: {
      type: Number,
      required: true,
    },
  },
  computed: {
    startPage() {
      if (this.currentPage === 1) {
        return 1;
      }

      if (this.currentPage === this.totalPages) { 
        return this.totalPages - this.maxVisibleButtons + 1;
      }
      
      return this.currentPage - 1;
    },
    endPage() {
      return Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages);
    },
    pages() {
      const range = [];
      for (let i = this.startPage; i <= this.endPage; i+= 1 ) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage,
        });
      }

      return range;
    },
    isInFirstPage() {
      return this.currentPage === 1;
    },
    isInLastPage() {
      return this.currentPage === this.totalPages;
    },
  },
  methods: {
    isPageActive(page) {
      return this.currentPage === page;
    },
    onClickFirstPage() {
      this.$emit("pagechanged", 1);
    },
    onClickPreviousPage() {
      this.$emit("pagechanged", this.currentPage - 1);
    },
    onClickPage(page) {
      this.$emit("pagechanged", page);
    },
    onClickNextPage() {
      this.$emit("pagechanged", this.currentPage + 1);
    },
    onClickLastPage() {
      this.$emit("pagechanged", this.totalPages);
    },
  },
};
</script>
