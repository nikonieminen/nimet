<template>
  <div class="container mt-3">
    <h1>Sukunimet</h1>
    <table class="table">
      <thead>
        <tr>
          <th>Tunniste</th>
          <th>Nimi</th>
          <th class="text-right">Väestö</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="name in names.Sukunimet" :key="i">
          <td>
            {{ name.ID }}
          </td>
          <td>
            {{ name.Sukunimi }}
          </td>
          <td class="text-right">
            {{ name.Lukumäärä }}
          </td>
          <td class="text-right">
            <NuxtLink
              class="btn btn-sm btn-primary"
              :to="{ path: 'lastname', query: { lastname: name.ID } }"
            >
              Lista
            </NuxtLink>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<style>
.visited-link {
  background: #ccc;
}
.main-search {
  width: 100%;
}
.main-search input {
  width: 100%;
  border: 2px solid black;
}
.main-search label {
  font-weight: bold;
}
</style>
<script>
const finnish_names = require('@/static/names.json');

export default {
  data() {
    return {
      address: '',
      visited: [],
      page: 0,
      all_names: finnish_names,
    };
  },
  methods: {
    loadPrevious() {
      this.initial = true;
      this.page = parseInt(localStorage.getItem('page'));
      this.address = localStorage.getItem('address');
      this.visited = localStorage.getItem('visited');
      if (!this.visited) this.visited = [];
    },
    visit(name) {
      if (!Array.isArray(this.visited)) this.visited = [];
      this.visited.push(name.name);
      localStorage.setItem('visited', this.visited);
    },
    isVisited(name) {
      return this.visited.indexOf(name.name) >= 0;
    },
  },
  watch: {
    page: function () {
      if (this.page < 0) {
        this.page = 0;
      }
      localStorage.setItem('page', parseInt(this.page));
    },
    address: function () {
      localStorage.setItem('address', this.address);
      if (!this.initial) {
        localStorage.setItem('visited', []);
        localStorage.setItem('page', 0);
        this.visited = [];
        this.page = 0;
      }
      this.initial = false;
    },
  },
  computed: {
    names: function () {
      return finnish_names;
    },
  },
  mounted() {},
};
</script>
