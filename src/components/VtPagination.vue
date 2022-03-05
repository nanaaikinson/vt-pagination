<template>
  <div :class="styles.parent">
    <template v-if="datatable">
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
    datatable: {
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
    const { currentPage, perPage, totalItems, datatable } = toRefs(props);

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

    // Hooks
    console.log(styles);

    return {
      styles,
      datatable,
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
