<template>
  <div id="sort">
    <b-form-group>
      <b-form-radio
        size="lg"
        @change="sortRecipes"
        v-model="selected"
        :checked="selected1 === 'false'"
        value="Alphabetic"
        class="a"
      >
        Alphabetic</b-form-radio
      >
      <b-form-radio
        size="lg"
        @change="sortRecipes"
        v-model="selected"
        :checked="!selected1"
        value="Calories"
        class="b"
        >Calories</b-form-radio
      >
    </b-form-group>
    <!--<div class="mt-3">
      Selected: <strong>{{ selected }}</strong>
    </div>-->
  </div>
</template>

<script>
export default {
  name: "sort-search",
  data() {
    return {
      selected: "",
    };
  },
  props: ["selected1"],
  methods: {
    sortRecipes() {
      this.$emit("sort-recipes", this.selected);
    },
  },
  /*created() {
    console.log(this.recipes);
  },*/
};
</script>

<style scoped>
input {
  margin-right: 10px;
}
p {
  text-align: center;
}
.a,
.b {
  padding: 10px 0;
}
</style>