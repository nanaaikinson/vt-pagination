# vt-pagination

[![npm (scoped)](https://img.shields.io/npm/v/@nanaaikinson24/vt-pagination)](https://www.npmjs.com/package/@nanaaikinson24/vt-pagination)
[![npm bundle size (scoped)](https://img.shields.io/bundlephobia/minzip/@nanaaikinson24/vt-pagination)](https://bundlephobia.com/result?p=@nanaaikinson24/vt-pagination@latest)
[![NPM](https://img.shields.io/npm/l/@nanaaikinson24/vt-pagination)](https://github.com/nanaaikinson/vt-pagination/blob/main/LICENSE)

---

vt-pagination is a lightweight Vue 3 tailwind pagination component.

## Usage

First install the package using your preferred package manager

```sh
npm install --save @nanaaikinson24/vt-pagination
```

```sh
yarn add @nanaaikinson24/vt-pagination
```

then you can import the component with its styling like so

```vue

<script>
import {VtPagination} from "@nanaaikinson24/vt-pagination";
import "@nanaaikinson24/vt-pagination/dist/style.css";
</script>
```

## Component Reference

#### Props

| Prop Name   | Description | Required | Type    |
|:------------|:------------|:---------|:--------|
| currentPage |             | true     | number  |
| perPage     |             | true     | number  |
| totalItems  |             | true     | number  |
| withTable   |             | false    | boolean |

## Full usage

#### JavaScript

```vue

<template>
  <div>
    <VtPagination
        :current-page="currentPage"
        :per-page="perPage"
        :total-items="totalItems"
        v-on:update:currentPage="onChangeCurrentPage"
    />
  </div>
</template>

<script>
import {defineComponent, ref} from "vue";
import {VtPagination} from "@nanaaikinson24/vt-pagination";
import "@nanaaikinson24/vt-pagination/dist/style.css";

export default defineComponent({
  name: "MyComponent",
  components: {VtPagination},
  setup() {
    const currentPage = ref(1);
    const perPage = ref(20);
    const totalItems = ref(2000);

    const onChangeCurrentPage = (page) => {
      currentPage.value = page;
    }

    return {currentPage, perPage, totalItems, onChangeCurrentPage}
  }
})
</script>
```

#### Typescript

```vue

<template>
  <div>
    <VtPagination
        :current-page="currentPage"
        :per-page="perPage"
        :total-items="totalItems"
        v-on:update:currentPage="onChangeCurrentPage"
    />
  </div>
</template>

<script lang="ts">
import {defineComponent, ref} from "vue";
import {VtPagination} from "@nanaaikinson24/vt-pagination";
import "@nanaaikinson24/vt-pagination/dist/style.css";

export default defineComponent({
  name: "MyComponent",
  components: {VtPagination},
  setup() {
    const currentPage = ref<number>(1);
    const perPage = ref<number>(20);
    const totalItems = ref<number>(2000);

    const onChangeCurrentPage = (page: number) => {
      currentPage.value = page;
    }

    return {currentPage, perPage, totalItems, onChangeCurrentPage}
  }
})
</script>
```

#### Simplified version

```vue

<template>
  <div>
    <VtPagination
        v-model:current-page="currentPage"
        :per-page="perPage"
        :total-items="totalItems"
    />
  </div>
</template>

<script setup lang="ts">
import {ref} from "vue";
import {VtPagination} from "@nanaaikinson24/vt-pagination";
import "@nanaaikinson24/vt-pagination/dist/style.css";

const currentPage = ref<number>(1);
const perPage = ref<number>(20);
const totalItems = ref<number>(2000);
</script>
```

## Todo

- [x] Publish v1
- [x] Update the Docs (README.md)
- [ ] Add go to page functionality
- [ ] Enable styling to be updated



