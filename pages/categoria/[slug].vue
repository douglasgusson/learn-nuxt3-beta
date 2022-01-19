<script lang="ts" setup>
import gqlClient from "~~/graphql/client";
import { GetCategoryBySlug } from "~~/graphql/queries";

const route = useRoute();
const slug = route.params.slug;

const { data } = await useLazyAsyncData(`category-${slug}`, () =>
  gqlClient.request(GetCategoryBySlug, { slug })
);

useMeta({
  title: `ReceitasApp`,
});
</script>

<template>
  <section class="hero is-link">
    <div class="hero-body">
      <div class="container has-text-centered">
        <h1 class="title">{{ data.category.name }}</h1>
      </div>
    </div>
  </section>

  <div class="container">
    <section class="section">
      <nav class="breadcrumb" aria-label="breadcrumbs">
        <ul>
          <li>
            <NuxtLink to="/">Página inicial</NuxtLink>
          </li>
          <li class="is-active">
            <a href="#" aria-current="page">{{ data.category.name }}</a>
          </li>
        </ul>
      </nav>
    </section>

    <section class="section">
      <div class="columns is-desktop is-centered">
        <div
          class="column is-one-quarter"
          v-for="recipe in data.category.recipes"
          :key="recipe.slug"
        >
          <div class="card">
            <div class="card-image">
              <figure class="image is-16by9">
                <img
                  :src="recipe.photo.url"
                  :width="recipe.photo.width"
                  :height="recipe.photo.height"
                  :alt="recipe.name"
                />
              </figure>
            </div>
            <div class="card-content">
              <div class="media">
                <div class="media-content">
                  <h1 class="title is-4">
                    <NuxtLink :to="`/receita/${recipe.slug}/`">
                      {{ recipe.name }}
                    </NuxtLink>
                  </h1>
                </div>
              </div>

              <div class="content">
                <p>{{ recipe.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
