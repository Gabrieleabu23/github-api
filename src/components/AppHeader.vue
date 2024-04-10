<script>
import { store } from "../store";


export default {
  name: "AppHeader",
  data() {
    return {
      store,
      timer: null,
      elapsedTime:0
    };
  },
  methods: {
    debounce(){
      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.elapsedTime = 0;
        this.checkClick();
      }, 700); 
      this.elapsedTime++;
    },
    checkClick() {
      // console.log(this.store.searchInput);
      if(this.store.searchInput.trim().length <3){
        document.getElementById('minLength').classList.remove('d-none');
      }else{
        document.getElementById('minLength').classList.add('d-none');
        this.store.TypeSearch = document.getElementById("typeSearch").value;
        if (this.store.clicked) {
          this.store.clicked = 0;
        } else {
          this.store.clicked = 1;
        }
      }
      
      // console.log('cliccato!');
    },
  },

};
</script>

<template>
  <div class="container">
    <div class="d-flex justify-content-between">
      <div>
        <input type="text" v-model="this.store.searchInput" @keyup.enter="checkClick()" @input="debounce()"/>
        <button @click="checkClick">CERCA</button>
        <span id="minLength" class="d-none text-danger">la parola da ricercare deve essere almeno di 3 caratteri</span>
      </div>
      <select name="typeSearch" id="typeSearch" @change="checkClick()" >
        <option disabled>Seleziona repo/utenti</option>
        <option value="Repository" selected>Repository</option>
        <option value="users">Users</option>
        <!-- <option value="" disabled selected>Seleziona repo/utenti</option> -->
      </select>
    </div>
  </div>
</template>

<style></style>
