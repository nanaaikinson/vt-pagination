<script setup lang="ts">
import { ref } from "vue";
import { VtPagination } from ".";

const currentPage = ref<number>(1);
</script>

<template>
  <VtPagination v-bind:current-page="currentPage" :per-page="10" :total-items="1000" :datatable="true" />
</template>
