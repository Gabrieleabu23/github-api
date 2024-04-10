<script>
import { store } from "../store";
import axios from "axios";
import config from "../config";
export default {
  name: "CallApi",
  data() {
    return {
      store,
      arrayApi: [],
      isLoading: false,
      currentPage: 1,
      totalPages: 1,
    };
  },
  methods: {
    async getRepo(pageNumber = 1) {
      this.isLoading = true;

      try {
        const response = await axios.get(
          "https://api.github.com/search/repositories",
          {
            params: {
              q: this.store.searchInput,
              page: pageNumber,
              per_page: 6, // Impostiamo il numero di risultati per pagina a 10
              order: "desc",
            },
            headers: {
              Authorization: `${config.githubToken}`,
              "X-GitHub-Api-Version": "2022-11-28",
            },
          }
        );
        this.arrayApi = response.data.items;
        this.totalPages = Math.ceil(response.data.total_count / 6); // Calcoliamo il numero totale di pagine
      } catch (error) {
        console.error("Errore durante il recupero dei dati:", error);
      } finally {
        this.isLoading = false;
        this.showPagination();
      }
    },
    showPagination() {
      if (this.isLoading) {
        document.getElementById("pagination").classList.add("d-none"); // Nascondi la paginazione durante il caricamento
      } else {
        setTimeout(() => {
          document.getElementById("pagination").classList.remove("d-none"); // Mostra la paginazione quando i dati sono stati caricati
        }, 500);
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.getRepo(this.currentPage);
        this.showPagination();
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.getRepo(this.currentPage);
        this.showPagination();
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
    "store.TypeSearch": function () {
      this.getRepo();
      // Esegui altre azioni qui in base al nuovo valore
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
  <div v-if="!isLoading" class="container">
    <div class="row w-75 mx-auto mt-4">
      <div
        class="col-12 col-md-6 col-lg-4 p-2"
        v-for="repo in arrayApi"
        :key="repo.id"
      >
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

            <div class="card-text paragraph-height overflow-hidden mb-0">
              <p v-if="repo.description" class="mb-0">
                {{ repo.description }}
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
  <div v-else class="text-center">
    <div class="spinner-border" role="status">
      <span class="sr-only">Loading...</span>
    </div>
    <p>Caricamento...</p>
  </div>
  <div id="pagination" class="d-none">
    <button @click="previousPage" :disabled="currentPage === 1">
      Indietro
    </button>
    <span>Pagina {{ currentPage }} di {{ totalPages }}</span>
    <button @click="nextPage" :disabled="currentPage === totalPages">
      Avanti
    </button>
  </div>
</template>

<style>
.card-body {
  height: 20rem;
}
.paragraph-height {
  max-height: 70px; /* Altezza massima del paragrafo */
  overflow-y: auto; /* Scroll verticale se il testo supera l'altezza massima */
}

.loader {
  position: fixed;
  z-index: 9999;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.spinner-border {
  width: 3rem;
  height: 3rem;
}
</style>
