<template>
  <div class="search">
    <div class="searchField">
      <input type="text" placeholder="Search for Food" v-model="inputVal" />
      <button @click="getRecipe()">Search</button>
    </div>
    <hr />
    <p v-show="selection != ''" style="text-align: center">
      Sorted by {{ selection }}
    </p>

    <div v-if="loading">Loading...</div>
    <div class="content" v-else>
      <div class="filterSearch">
        <SortSearch @sort-recipes="sorting" />
      </div>
      <div class="recipesWrapper">
        <div class="recipes" v-if="exist">
          <div
            class="oneRecipe"
            v-for="(recipe, index) in recipes"
            :key="index"
          >
            <h4 class="mealtype" v-if="recipe.recipe.mealType">
              {{ recipe.recipe.mealType[0] }}
            </h4>
            <h2 class="recipeName">{{ recipe.recipe.label }}</h2>
            <img
              class="recipeImage"
              :src="recipe.recipe.image"
              alt="Recipe Image"
            />
            <ul>
              <h3>Ingredients:</h3>
              <li
                v-for="(ingr, index) in recipe.recipe.ingredientLines"
                :key="index"
              >
                {{ ingr }}
              </li>
            </ul>
            <p v-if="recipe.recipe.calories" class="calories">
              Calories:
              {{ Math.round(recipe.recipe.calories) }} kcal
            </p>
          </div>
        </div>
        <div v-else>
          <p>Sorry, nothing found.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SortSearch from "./Sort.vue";

export default {
  name: "Search",
  components: {
    SortSearch,
  },
  props: {},
  data() {
    return {
      appId: process.env.VUE_APP_ID,
      api_key: process.env.VUE_APP_API,
      inputVal: "",
      calories: "",
      recipes: [],
      label: "",
      ingredients: [],
      exist: true,
      loading: false,
      selection: "",
    };
  },
  created() {
    axios
      .get(
        "https://api.edamam.com/api/recipes/v2?type=public&q=burger&app_id=" +
          this.appId +
          "&app_key=" +
          this.api_key
      )
      .then((res) => {
        this.recipes = res.data.hits;
      })
      .catch((e) => {
        console.error(e);
      });
  },
  mounted() {
    window.addEventListener("keyup", (event) => {
      if (event.keyCode === 13) {
        this.getRecipe();
      }
    });
  },
  methods: {
    async getRecipe() {
      await axios
        .get(
          "https://api.edamam.com/api/recipes/v2?type=public&q=" +
            this.inputVal +
            "&app_id=" +
            this.appId +
            "&app_key=" +
            this.api_key
        )
        .then((res) => {
          this.loading = false;
          this.recipes = res.data.hits;
          this.inputVal = "";
          if (res.data.hits.length > 0) {
            this.exist = true;
          } else {
            this.exist = false;
          }
          console.log(res.data.hits);
        })
        .catch((e) => {
          console.error(e);
        });
    },
    sorting(selected) {
      if (selected === "Alphabetic") {
        this.selection = selected;
        this.recipes.sort((a, b) => {
          if (a.recipe.label < b.recipe.label) return -1;
          if (a.recipe.label > b.recipe.label) return 1;
          return 0;
        });
      }
      if (selected === "Calories") {
        this.selection = selected;
        this.recipes.sort((a, b) => {
          if (a.recipe.calories < b.recipe.calories) return -1;
          if (a.recipe.calories > b.recipe.calories) return 1;
          return 0;
        });
      }
    },
  },
};
</script>

<style scoped>
.content {
  display: grid;
  grid-template-columns: minmax(150px, 15%) 1fr;
}
.search {
  margin: 2% 0;
}
.searchField {
  display: block;
  margin: 0 auto;
  text-align: center;
}
input {
  width: 450px;
  height: 30px;
  border-radius: 0%;
  border: none;
  border-bottom: 1.5px solid #58b989;
  outline: none;
}
button {
  background: #58b989;
  color: #fff;
  border-radius: 0%;
  border: none;
  height: 32px;
  width: 80px;
  font-weight: 600;
}
button:hover {
  background: #47946d;
}

ul {
  list-style-position: outside;
}
li {
  text-align: left;
}
.filterSearch {
  margin: 22% 10px;
  max-height: 500px;
}
.recipes {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  grid-gap: 20px;
  justify-content: center;
  margin: 4% 10px;
}
.oneRecipe {
  border: 1px solid #ccc;
  box-shadow: 0px 1px 2px 0px rgba(0, 0, 0, 0.5);
  padding: 20px;
  border-radius: 12px;
  background: #fff;
}
.recipeName {
  text-align: center;
  font-size: 18px;
  font-weight: 600;
}
.mealtype {
  text-align: center;
  display: block;
  margin: 2% auto;
  color: #fff;
  background: #4ea57a;
  border-radius: 12px;
  max-width: 150px;
  font-size: 16px;
  padding: 4px 0;
}
h3 {
  font-size: 18px;
}
.recipeImage {
  max-width: 300px;
  display: block;
  margin: 0 auto;
  padding: 20px 0;
}

.calories {
  text-align: center;
  color: #fff;
  background: #a5624e;
  border-radius: 12px;
  font-size: 16px;
  padding: 4px 0;
}

@media only screen and (max-width: 600px) {
  .content {
    display: grid;
    grid-template-columns: 1fr;
  }
  .search {
    margin: 10% 0;
  }
  input {
    width: 250px;
    border-bottom: 1.5px solid #58b989;
    outline: none;
  }
  .oneRecipe {
    max-height: 600px;
    overflow-y: scroll;
  }
}
</style>
