<template>
  <div :class="styles.parent">
    <template v-if="withTable">
      <div :class="styles['showing-parent']">
        <span>Showing</span>
        <span :class="styles['showing-highlight']">{{ pageFrom }}</span>
        <span>to</span>
        <span :class="styles['showing-highlight']">{{ pageTo }}</span>
        <span>of</span>
        <span :class="styles['showing-highlight']">{{ totalItems }}</span>
        <span>results</span>
      </div>
    </template>

    <nav aria-label="Pagination">
      <ul role="menubar" :class="styles.nav">
        <!-- Previous -->
        <li class="">
          <button
            role="menuitem"
            :class="[styles['page-item'], styles['prev'], isFirstPage ? styles.disabled : '']"
            :disabled="isFirstPage"
            @click.prevent="handlePageChange(currentPage - 1)"
          >
            Previous
          </button>
        </li>
        <!-- Previous -->

        <!-- Page 1 -->
        <li v-if="hasFirst()">
          <button role="menuitem" :class="styles['page-item']" @click.prevent="handlePageChange(1)">1</button>
        </li>
        <!-- Page 1 -->

        <!-- Elipsis -->
        <li v-if="hasFirst()" role="separator">
          <span :class="styles['page-item']">...</span>
        </li>
        <!-- Elipsis -->

        <!-- Pages -->
        <li v-for="(page, index) in calculatedPages" :key="index" aria-current="page">
          <button
            role="menuitem"
            class=""
            :class="[styles['page-item'], currentPage === page ? `${styles['active']} ${styles.disabled}` : '']"
            :disabled="currentPage === page"
            @click.prevent="handlePageChange(page)"
          >
            {{ page }}
          </button>
        </li>
        <!-- Pages -->

        <!-- Elipsis -->
        <li v-if="hasLast()" role="separator">
          <span :class="styles['page-item']">...</span>
        </li>
        <!-- Elipsis -->

        <!-- Last page -->
        <li v-if="hasLast()">
          <button role="menuitem" :class="styles['page-item']" @click.prevent="handlePageChange(totalPages)">
            {{ totalPages }}
          </button>
        </li>
        <!-- Last page -->

        <!-- Next page -->
        <li>
          <button
            role="menuitem"
            :class="[styles['page-item'], styles.next, isLastPage ? styles.disabled : '']"
            :disabled="isLastPage"
            @click.prevent="handlePageChange(currentPage + 1)"
          >
            Next
          </button>
        </li>
        <!-- Next page -->
      </ul>
    </nav>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref, toRefs } from "vue";
import styles from "@/components/VtPagination.module.css";

export default defineComponent({
  name: "VtPagination",
  props: {
    currentPage: {
      type: Number,
      required: true,
    },
    perPage: {
      type: Number,
      required: true,
    },
    totalItems: {
      type: Number,
      required: true,
    },
    withTable: {
      type: Boolean,
      required: false,
      default: () => false,
    },
  },
  emits: ["update:currentPage"],
  setup(props, { emit }) {
    // Data
    const pageRange = ref<number>(1);

    // Props
    const { currentPage, perPage, totalItems, withTable } = toRefs(props);

    // Computed
    const totalPages = computed(() => Math.ceil(totalItems.value / perPage.value));
    const pageFrom = computed(() => (currentPage.value && perPage.value ? (currentPage.value - 1) * perPage.value + 1 : 0));
    const pageTo = computed(() => {
      const end = currentPage.value * perPage.value;
      return totalItems.value < end ? totalItems.value : end;
    });
    const isFirstPage = computed(() => {
      return currentPage.value === 1;
    });
    const isLastPage = computed(() => {
      return currentPage.value >= totalPages.value;
    });
    const rangeStart = computed(() => {
      const start = currentPage.value - pageRange.value;
      return start > 0 ? start : 1;
    });
    const rangeEnd = computed(() => {
      const end = currentPage.value + pageRange.value;
      return end < totalPages.value ? end : totalPages.value;
    });
    const calculatedPages = computed(() => {
      const pages = [];
      for (let i = rangeStart.value; i <= rangeEnd.value; i++) {
        pages.push(i);
      }
      return pages;
    });
    // Methods
    const hasFirst = () => {
      return rangeStart.value !== 1;
    };
    const hasLast = () => {
      return rangeEnd.value < totalPages.value;
    };
    const handlePageChange = (page: number) => {
      if (page > 0 && page <= totalPages.value) {
        emit("update:currentPage", page);
      }
    };

    return {
      styles,
      withTable,
      totalPages,
      pageFrom,
      pageTo,
      isFirstPage,
      isLastPage,
      rangeStart,
      rangeEnd,
      calculatedPages,
      hasFirst,
      hasLast,
      handlePageChange,
    };
  },
});
</script>
