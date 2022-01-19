<script lang="ts" setup>
import gqlClient from "~~/graphql/client";
import { GetRecipes } from "~~/graphql/queries";

const { data } = await useLazyAsyncData("recipes", () =>
  gqlClient.request(GetRecipes)
);

useMeta({
  title: "ReceitasApp",
  description: "ReceitasApp é um site de receitas de cozinha",
});
</script>

<template>
  <section class="hero is-link is-halfheight">
    <div class="hero-body">
      <div class="container has-text-centered">
        <h1 class="title">
          <strong>ReceitasApp</strong> é um site de receitas de cozinha
        </h1>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="columns is-desktop is-centered">
      <div
        class="column is-one-quarter"
        v-for="recipe in data.recipes"
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
</template>
