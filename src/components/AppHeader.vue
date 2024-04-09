<script >
import {store} from '../store';

export default {
    name: 'AppHeader',
    data() {
        return {
            store,
        }
    },
    methods:{
        checkClick(){
            // console.log(this.store.searchInput);
            if(this.store.clicked){
                this.store.clicked=0;
            }else{

                this.store.clicked=1;
            }
            // console.log('cliccato!');
        }
    },

}

</script>

<template>
  <div class="container">
       <input type="text" v-model="this.store.searchInput">
       <button @click="checkClick">CERCA</button>
  </div>
</template>

<style >

</style>
