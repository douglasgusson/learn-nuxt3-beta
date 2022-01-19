<script lang="ts" setup>
import gqlClient from "~~/graphql/client";
import { GetCategoriesMenu } from "~~/graphql/queries";

const isActiveMenu = ref(false);

const { data } = await useLazyAsyncData("categories-menu", () =>
  gqlClient.request(GetCategoriesMenu)
);
</script>

<template>
  <nav class="navbar">
    <div class="container">
      <div class="navbar-brand">
        <NuxtLink class="navbar-item" to="/">
          <span class="is-size-5 has-text-weight-semibold">ReceitasApp</span>
        </NuxtLink>
        <span
          @click="isActiveMenu = !isActiveMenu"
          class="navbar-burger burger"
          :class="{ 'is-active': isActiveMenu }"
        >
          <span></span>
          <span></span>
          <span></span>
        </span>
      </div>
      <div
        id="navbarMenu"
        class="navbar-menu"
        :class="{ 'is-active': isActiveMenu }"
      >
        <div class="navbar-end">
          <NuxtLink
            v-for="category in data.categories"
            :to="`/categoria/${category.slug}/`"
            class="navbar-item is-active is-size-5 has-text-weight-semibold"
          >
            {{ category.name }}
          </NuxtLink>
        </div>
      </div>
    </div>
  </nav>
</template>
