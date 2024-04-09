<script>
import { store } from "../store";
import axios from "axios";
export default {
  name: "CallApi",
  data() {
    return {
      store,
      arrayApi: [],
    };
  },
  methods: {
    getRepo() {
      console.log("sono in getRepo()");
      console.log(this.store.searchInput);
      if (
        this.store.searchInput.trim() != "" &&
        this.store.TypeSearch == "Repository"
      ) {
        const data = {
          params: {
            q: this.store.searchInput,
          },
          headers: {
            Authorization: `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`,
            "X-GitHub-Api-Version": "2022-11-28",
          },
        };

        axios
          .get("https://api.github.com/search/repositories", data)
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
      } else if (
        this.store.searchInput.trim() != "" &&
        this.store.TypeSearch == "users"
      ) {
        const data = {
          params: {
            q: `${this.store.searchInput}`,
          },
          headers: {
            Authorization: `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`,
            "X-GitHub-Api-Version": "2022-11-28",
          },
        };
        axios
          .get("https://api.github.com/search/users", data)
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
    },
  },
  watch: {
    checkifClick(newValue, oldValue) {
      if (newValue === 1 || newValue !== oldValue) {
        this.getRepo();
      }
      console.log("valore nuovo:", newValue);
      console.log("valore vecchio:", oldValue);
    },
  },
  computed: {
    checkifClick() {
      console.log("computed");
      console.log("valore attuuale:", this.store.clicked);
      return this.store.clicked;
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="row w-75 mx-auto mt-4">
      <div class="col-4 p-2" v-for="repo in arrayApi">
        <div class="card rounded-4">
          <div class="bg-dark d-flex justify-content-center rounded-top">
            <img
              :src="repo.avatar_url ? repo.avatar_url : repo.owner.avatar_url"
              class="card-img-top rounded-circle p-2 img-fluid"
              alt="..."
              style="width: 12rem"
            />
          </div>
          <div class="card-body d-flex flex-column">
            <h5 class="card-title">
              {{ repo.full_name ? repo.full_name : repo.login }}
            </h5>

            <div class="paragraph_height overflow-hidden">
              <p class="card-text">
                <span v-if="repo.description">{{ repo.description }}</span>
              </p>
            </div>
            <h4 v-if="repo.stargazers_count">
              <i class="fa-solid fa-star"></i> {{ repo.stargazers_count }}
            </h4>
            <h4 v-if="repo.open_issues_count">
              <i class="fa-solid fa-circle-exclamation"></i>
              {{ repo.open_issues_count }}
            </h4>
            <h4 v-if="repo.type">
              Profilo :
              {{ repo.type == "Organization" ? "Organizzazione" : "Utente" }}
            </h4>
            <a
              :href="repo.html_url"
              target="_blank"
              class="btn btn-primary mt-auto"
              >{{ repo.type ? "Vai al profilo" : "Vai alla repo" }}</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.paragraph_height{
    max-height: 80px;
}
.card-body {
  height: 200px;
}
</style>
