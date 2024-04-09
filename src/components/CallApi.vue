<script >
import {store} from '../store';
import axios from 'axios';
export default {
    name: 'CallApi',
    data() {
        return {
            store,
            arrayApi:[]
        }
    },
    methods:{
        getRepo() {
            console.log('sono in getRepo()');
            console.log(this.store.searchInput);
            const data = {
                    params: {
                        q: this.store.searchInput,
                    },
                    headers: {
                        "Authorization": `Bearer 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`,
                        "X-GitHub-Api-Version": "2022-11-28"
                    }
                };
            if(this.store.searchInput.trim() != ''){

                axios.get('https://api.github.com/search/repositories', data)
                    .then((res) => {
                        // res.data.forEach(element => {
                        //     this.repositories.push(element.name);
                        // });
                        // console.log(res.data.items);
                        this.arrayApi = res.data.items;
                        if (this.arrayApi.length > 0 && this.store.cliccato == 1) {
                            this.store.cliccato = 0;
                        }
                        console.log(this.arrayApi);
                    })
                    .catch((error) => {
                        console.error("Error fetching data from first API:", error);
                    });
                
            }
        }
    },
    watch:{
        checkifClick(newValue,oldValue) {
            if((newValue === 1 || newValue !== oldValue )){
                this.getRepo();
            }
            console.log('valore nuovo:',newValue);
            console.log('valore vecchio:',oldValue);
    }
    },
    computed:{
        checkifClick(){
            console.log('computed');
            console.log('valore attuuale:',this.store.clicked);
            return this.store.clicked;
        }
    }
}

</script>

<template>
  <div class="container">
        <div class="row">
            <div class="col-4" v-for="repo in arrayApi">
                {{ repo.name }}
            </div>
        </div>
  </div>
</template>

<style >

</style>
