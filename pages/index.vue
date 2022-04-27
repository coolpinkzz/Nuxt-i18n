<template>
  <div>
    <nav>
      <div class="menu">
        <ul>
          <li v-for="menu in allMenu" :key="menu.id">
            {{ menu.name }}
          </li>
        </ul>
      </div>
      <div class="selectLang">
        <select @change="handleLang" v-model="$i18n.locale">
          <option
            v-on:click="handleLang"
            v-for="lang in fetchAllLang"
            :key="lang.code"
            :value="lang.code"
          >
            {{ lang.name }}
          </option>
        </select>
      </div>
    </nav>

    <div class="container">
      <h1 class="title">{{ $t("message") }}</h1>
      <!-- some code -->
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      fetchAllLang: null,
      langSelected: "",
      allMenu: null,
    };
  },
  methods: {
    handleLang(event) {
      axios
        .get(`https://api.bookcab.fr/menus?_locale=${event.target.value}`)
        .then((response) => {
          this.allMenu = response.data;
        });
    },
  },
  mounted() {
    axios
      .get("https://api.bookcab.fr/i18n/locales")
      .then((response) => {
        this.fetchAllLang = response.data;
        this.langSelected = response.data[0].name;
      })
      .catch((err) => console.log(err));
    axios.get("https://api.bookcab.fr/menus?_locale=en").then((response) => {
      this.allMenu = response.data;
    });
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
nav {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  background-color: #d5d8dc;
}
.menu ul {
  display: flex;
  justify-content: space-between;
  width: 500px;
}
.menu ul li {
  list-style: none;
}

.container {
  max-width: 75%;
  margin: auto;
}
.selectLang {
  text-align: right;
}
.selectLang select {
  border: none;
  background: none;
}
</style>
