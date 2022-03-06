### vt-pagination

---

VtPagination is a vue3 tailwindcss pagination component

#### Installation

```sh
npm install --save @nanaaikinson24/vt-pagination
```

```sh
yarn add @nanaaikinson24/vt-pagination
```

#### Usage

```js
<script setup lang="ts">
import { VtPagination } from "@nanaaikinson24/vt-pagination";
import "@nanaaikinson24/vt-pagination/dist/style.css";
import { ref } from "vue";

const currentPage = ref<number>(1);
</script>

<template>
  <div>
    <VtPagination
      v-model:current-page="currentPage"
      with-table
      :per-page="10"
      :total-items="1000"
    />
  </div>
</template>
```
