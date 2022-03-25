<template>
  <h1>Recipes</h1>

  <div class="bar">
    <img src="../assets/plus.png" alt="" />
    <div class="form">
      <label>Name: </label>
      <input type="text" v-model="recipe" />

      <label>Ingredients: </label>
      <input type="text" v-model="ingredients" @keyup="key(ingredients)" />

      <label>Gluten free: </label>
      <input type="checkbox" v-model="free" />
    </div>
  </div>
  <div class="live">
    <form @submit.prevent="submit">
      <span id="span" v-if="recipe">Recipe: {{ recipe }}</span>
      <span id="span" v-if="ingredientsArr.length">Ingredients:</span>
      <span
        style="
          background: pink;
          color: black;
          padding: 10px;
          border-radius: 20px;
          margin: 10px;
        "
        v-for="item in ingredientsArr"
        :key="item"
        @click="remove(item)"
        >{{ item }}</span
      >
      <span id="span" v-if="free">Gluten free: {{ free }}</span>
      <button class="save" v-if="recipe && ingredientsArr.length">save</button>
    </form>
  </div>

  <div v-if="db.length">
    <form class="search">
      <input type="text" v-model="search" @keyup="find(search)"/>
    </form>
  </div>

  <div class="showcase">
    <div v-for="item in db" :key="item">
    <div class="item" v-if="item.toggle">
      <p>{{ item.recipe }}</p>
      <img id="img-right" src="../assets/gluten.png" alt="" v-if="item.free" />
      <img
        id="img-left"
        src="../assets/remove.png"
        alt=""
        @click="removeItem(item)"
      />
    </div>
    </div>
  </div>
</template>
<script>
import database from "../../data/database.json";
export default {
  data() {
    return {
      db: database.recipes,
      recipe: null,
      ingredients: null,
      free: null,
      ingredientsArr: [],
      toggle: null
    };
  },
  methods: {
    submit() {
      let object = {
        recipe: this.recipe,
        ingredients: this.ingredientsArr,
        free: this.free,
        toggle: true
      };
      this.recipe = "";
      this.ingredientsArr = [];
      this.free = "";
      this.db.push(object);
    },
    key(e) {
      if (e.includes(" " || ",")) {
        this.ingredientsArr.push(e);
        this.ingredients = "";
      }
    },
    remove(e) {
      this.ingredientsArr.shift(e);
    },
    removeItem(e) {
      this.db.shift(e);
    },
    find(e) {
      for (let i = 0; i < this.db.length; i++) {
        this.db[i].toggle = this.db[i].recipe.includes(e)
      }
    },
  },
};
</script>

<style>
.search {
  background: crimson;
  color: #ddd;
  font-size: 25px;
  padding: 20px;
  text-align: left;
  justify-content: space-between;
}

.item #img-right {
  position: absolute;
  right: 10px;
  top: 18px;
}
.item #img-left {
  position: absolute;
  left: 10px;
  top: 18px;
}
.item {
  position: relative;
  background-image: url("../assets/plant.jpg");
  width: 400px;
  height: 250px;
  margin: 20px 50px;
  border-radius: 20px;
  overflow: hidden;
}
.item p {
  position: absolute;
  top: 0;
  width: 100%;
  padding: 20px;
  font-size: 30px;
  background: pink;
  border-radius: 0 0 20px 20px;
}
.bar {
  background: #41b883;
  margin: 20px 0 0 0;
  height: 70px;
  position: relative;
}
.bar img {
  position: absolute;
  left: 10px;
  top: 10px;
}
.form {
  height: 100%;
  display: flex;
  width: 50%;
  margin: 0 auto;
  justify-content: space-around;
  align-items: center;
}
label {
  font-size: 30px;
}
input[type="text"] {
  font-size: 30px;
  width: 200px;
  padding: 0 5px;
}
input[type="checkbox"] {
  cursor: pointer;
}
.live {
  background: #34495e;
  color: #ddd;
  font-size: 25px;
  padding: 20px;
  text-align: left;
  position: relative;
}
.live #span {
  margin: 20px;
}
.save {
  position: absolute;
  right: 10px;
  top: 10px;
  background: #41b883;
  padding: 10px;
  border-radius: 30px;
  border: none;
  font-size: 25px;
}
.showcase {
  background: #ddd;
  margin: 50px;
  border-radius: 20px;
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
</style>
