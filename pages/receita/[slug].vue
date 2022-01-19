<script lang="ts" setup>
import gqlClient from "~~/graphql/client";
import { GetRecipeBySlug } from "~~/graphql/queries";

const route = useRoute();
const slug = route.params.slug;

const { data } = await useAsyncData(`recipe-${slug}`, () =>
  gqlClient.request(GetRecipeBySlug, { slug })
);

useMeta({
  title: `ReceitasApp`,
});
</script>

<template>
  <section class="hero is-primary">
    <div class="hero-body">
      <div class="container has-text-centered">
        <h1 class="title">{{ data.recipe.name }}</h1>
        <p>{{ data.recipe.description }}</p>
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
          <li>
            <NuxtLink :to="`/categoria/${data.recipe.category.slug}/`">{{
              data.recipe.category.name
            }}</NuxtLink>
          </li>
          <li class="is-active">
            <a href="#" aria-current="page">{{ data.recipe.name }}</a>
          </li>
        </ul>
      </nav>
    </section>

    <section class="section">
      <div class="columns">
        <div class="column">
          <div class="content is-medium">
            <figure class="image is-16by9">
              <img
                :src="data.recipe.photo.url"
                :width="data.recipe.photo.width"
                :height="data.recipe.photo.height"
                :alt="data.recipe.name"
              />
            </figure>
            <div v-html="data.recipe.ingredients.html"></div>
            <div v-html="data.recipe.preparation.html"></div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
