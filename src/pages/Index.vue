<template>
  <q-page class="q-pa-md">
    <div class="row">
      <div class="col-4 q-gutter-lg">
        <p>Wpisuj wszystko po angielsku.</p>

        <q-input
          filled
          v-model="query"
          label="Potrawa"
          hint="wpisz nazwe potrawy"
          :disable="isLoading"
          clearable
          counter
        />

        <q-select
          label="Ingridients"
          filled
          v-model="ingredients"
          use-input
          use-chips
          multiple
          hide-dropdown-icon
          input-debounce="0"
          new-value-mode="add-unique"
          hint="Po wpisaniu składnika wciśnij enter"
          :disable="isLoading"
        />

        <q-btn color="primary" @click="search()" :disable="isLoading"
          >szukaj
        </q-btn>
        <q-btn
          color="secondary"
          @click="showHelpModal = true"
          :disable="isLoading"
          >wyjaśnienie
        </q-btn>
        <p class="text-caption">Strona: {{ page }}</p>
      </div>
      <div class="col q-ml-md">
        <template v-if="!isLoading">
          <template v-if="recipes.length > 0" )>
            <q-card
              v-for="recipe in recipes"
              :key="recipe.href"
              class="my-card q-mb-md"
              flat
              bordered
            >
              <q-card-section horizontal>
                <q-card-section class="q-pt-xs">
                  <div class="text-h5 q-mt-sm q-mb-xs">{{ recipe.title }}</div>
                  <div class="q-gutter-xs">
                    <q-badge
                      v-for="ingredient in recipe.ingredients.split(', ')"
                      outline
                      color="secondary cursor-pointer"
                      :label="ingredient"
                      @click="ingredients.push(ingredient)"
                    />
                  </div>
                </q-card-section>

                <q-space></q-space>

                <q-card-section class="col-2">
                  <q-img
                    class="rounded-borders shadow-5"
                    :src="recipe.thumbnail"
                  />
                </q-card-section>
              </q-card-section>

              <q-separator />

              <q-card-actions>
                <q-btn @click="searchSimilar(recipe)" flat>
                  podobne
                </q-btn>
                <q-btn flat color="primary" @click="openUrl(recipe.href)">
                  otwórz przepis
                </q-btn>
              </q-card-actions>
            </q-card>
          </template>
          <template v-else>
            <q-banner rounded>
              <p>Brak przepisów do wyświetlenia.</p>
              <p>Wyszukaj by zobaczyć przepisy.</p>
            </q-banner>
          </template>
        </template>
        <template v-else>
          <div class="flex-center flex q-mt-xl">
            <q-spinner color="primary" size="3em" :thickness="10" />
          </div>
        </template>
        <q-page-sticky
          position="bottom-right"
          :offset="[18, 18]"
          v-if="recipes.length > 0"
        >
          <q-btn
            :disable="isLoading"
            fab
            icon="navigate_next"
            color="primary"
            class="text-black"
            @click="nextPage()"
          />
        </q-page-sticky>
      </div>
    </div>

    <q-dialog v-model="showHelpModal">
      <help-modal-carousel />
    </q-dialog>
  </q-page>
</template>

<script>
// https://material.io/resources/icons/
// https://github.com/axios/axios
// https://quasar.dev
// https://rapidapi.com/brianiswu/api/recipe-puppy

import { LoadingBar, openURL } from "quasar";
import axios from "axios";
import HelpModalCarousel from "../components/HelpModalCarousel";

export default {
  name: "PageIndex",
  components: { HelpModalCarousel },
  mounted() {
    LoadingBar.setDefaults({
      color: "primary",
      size: "15px",
      position: "bottom"
    });
  },
  data() {
    return {
      page: 1,
      query: "",
      ingredients: [],
      recipes: [],
      isLoading: false,
      showHelpModal: false
    };
  },
  methods: {
    openUrl(url) {
      openURL(url);
    },
    search(page = 1) {
      this.isLoading = true;
      this.page = page;
      axios
        .get("https://recipe-puppy.p.rapidapi.com", {
          params: {
            p: this.page,
            i: this.ingredients.join(", "),
            q: this.query
          },
          headers: {
            "x-rapidapi-host": "recipe-puppy.p.rapidapi.com",
            "x-rapidapi-key":
              "7ca40b8bcamshdb3a4dc2bd6ff75p18d114jsned0112c062ae"
          }
        })
        .then(({ data }) => {
          const { results } = data;
          this.recipes = results;
          this.isLoading = false;
        });
    },
    searchSimilar(recipe) {
      this.query = "";
      this.ingredients = recipe.ingredients.split(", ");
      this.search();
    },
    nextPage() {
      ++this.page;
      this.search(this.page);
    }
  }
};
</script>
