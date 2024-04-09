<script>
import { store } from "../store";

export default {
  name: "AppHeader",
  data() {
    return {
      store,
    };
  },
  methods: {
    checkClick() {
      // console.log(this.store.searchInput);
      if (this.store.clicked) {
        this.store.clicked = 0;
      } else {
        this.store.clicked = 1;
      }

      this.store.TypeSearch = document.getElementById("typeSearch").value;
      // console.log('cliccato!');
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="d-flex justify-content-between">
      <div>
        <input type="text" v-model="this.store.searchInput" />
        <button @click="checkClick">CERCA</button>
      </div>
      <select name="typeSearch" id="typeSearch">
        <option disabled>Seleziona repo/utenti</option>
        <option value="Repository" selected>Repository</option>
        <option value="users">Users</option>
        <!-- <option value="" disabled selected>Seleziona repo/utenti</option> -->
      </select>
    </div>
  </div>
</template>

<style></style>
