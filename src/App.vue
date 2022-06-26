<template>
  <main class="main-component">
    <h1 class="title">Coctails</h1>
    <div class="search-box">
      <input type="text" placeholder="Seach Raksi" v-model="Raksi" />
      <hr class="underline" />
    </div>
    <div class="sort">
      <i
        class="fa-solid fa-arrow-up-a-z up-arrow"
        @click="
          {
            this.sortDirection = false;
            sortItem;
          }
        "
      ></i>
      <i
        class="fa-solid fa-arrow-down-z-a down-arrow"
        @click="
          (e) => {
            this.sortDirection = true;
            sortItem;
          }
        "
      ></i>
    </div>
    <section>
      <template v-if="searchResult.length == 0">
        <h3 class="no-result">The is no Cocktail called {{ this.Raksi }}</h3>
      </template>
      <template v-else>
        <article class="component">
          <div
            class="card"
            v-for="cocktail in cocktailSortedSearchedList"
            v-bind:key="cocktail.idDrink"
            style="width: 18rem"
          >
            <img
              :src="cocktail.strDrinkThumb"
              alt="test"
              class="card-img-top"
            />
            <div class="card-body">
              <h1 class="card-title">
                {{ cocktail.strGlass }}
              </h1>
              <p class="card-text">
                Den Galliano-Likör über Eis gießen. Füllen Sie den Rest des
                Glases mit Ginger Ale und das ist alles, was dazu gehört. Du
                hast jetzt ein eigenes GG.
              </p>
            </div>
          </div>
        </article>
      </template>
    </section>
  </main>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      cocktailList: [],
      Raksi: "",
      cocktailSortedSearchedList: [],
      sortDirection: true,
    };
  },
  methods: {
    fetchCoctails: function () {
      axios
        .get("https://www.thecocktaildb.com/api/json/v1/1/search.php?s=")
        .then((response) => {
          this.cocktailList = response.data.drinks;
        });
    },
    handleSearch() {
      const searchedRakshi = this.cocktailList.filter((cocktail) => {
        return cocktail.strGlass
          .toLowerCase()
          .includes(this.Raksi.toLowerCase());
      });
      this.cocktailSortedSearchedList = searchedRakshi;
      return searchedRakshi;
    },
    handleClick() {
      this.cocktailSortedSearchedList = this.cocktailSortedSearchedList.sort(
        (a, b) => {
          console.log();
          if (this.sortDirection)
            return b.strGlass
              .toLowerCase()
              .localeCompare(a.strGlass.toLowerCase());
          else
            return a.strGlass
              .toLowerCase()
              .localeCompare(b.strGlass.toLowerCase());
        }
      );
    },
  },
  computed: {
    searchResult: function () {
      // const searchedRakshi = this.cocktailList.filter((cocktail) => {
      //   return cocktail.strGlass
      //     .toLowerCase()
      //     .includes(this.Raksi.toLowerCase());
      // });
      // return searchedRakshi;
      return this.handleSearch();
    },
    sortItem: function () {
      return this.handleClick();
    },
  },
  mounted() {
    this.fetchCoctails();
  },
};
</script>

<style>
.main-component {
  padding: 2rem;
  background-image: linear-gradient(
    to bottom right,
    rgb(255, 204, 153, 0.2),
    rgb(204, 0, 0, 0.2)
  );
  min-height: 100vh;
}
.title {
  display: flex;
  font-size: 5rem;
  justify-content: center;
  margin-bottom: 2rem;
}
.component {
  padding: 1rem;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2px;
}
.card {
  padding: 0.5rem;
  margin: 1rem;
}
.search-box {
  font-size: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.underline {
  font-size: large;
  padding: 2px;
}
.sort {
  display: flex;
  padding: 1rem;
  font-size: 2rem;
  justify-content: right;
}
.up-arrow {
  padding: 1rem;
}
.down-arrow {
  padding: 1rem;
}

input {
  outline: 0;
  border: 0;
}
.no-result {
  color: green;
  display: block;
}
</style>
