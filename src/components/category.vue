<script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        categories: [],
        tags: [],
        selectedCategory: null,
        selectedTags: [],
        posts: [],
        filteredPosts: [] // Inizializza filteredPosts come un array vuoto
      };
    },
    mounted() {
      // Effettua le chiamate API per ottenere categorie, tag e post
      axios.get('http://localhost:3000/api/categories')
        .then(response => {
          this.categories = response.data;
        })
        .catch(error => {
          console.error('Errore durante il recupero delle categorie:', error);
        });
  
      axios.get('http://localhost:3000/api/tags')
        .then(response => {
          this.tags = response.data;
        })
        .catch(error => {
          console.error('Errore durante il recupero dei tag:', error);
        });
  
      axios.get('http://localhost:3000/api/posts')
        .then(response => {
          this.posts = response.data.data;
          this.applyFilters(); // Applica i filtri quando i dati dei post sono disponibili
        })
        .catch(error => {
          console.error('Errore durante il recupero dei post:', error);
        });
    },
    methods: {
      applyFilters() {
        // Filtra i post in base alla categoria selezionata
        let filteredByCategory = this.posts;
        if (this.selectedCategory) {
          filteredByCategory = filteredByCategory.filter(post => post.categoryId === this.selectedCategory);
        }
  
        // Filtra i post in base ai tag selezionati
        let filteredByTags = filteredByCategory;
        if (this.selectedTags.length > 0) {
          filteredByTags = filteredByTags.filter(post => post.tags.some(tag => this.selectedTags.includes(tag.id)));
        }
  
        // Aggiorna l'array di post filtrati
        this.filteredPosts = filteredByTags;
      },
      submitFilters() {
        // Esegui ulteriori azioni quando viene fatto clic sul pulsante "Applica Filtri"
        console.log('Filtri applicati:', this.selectedCategory, this.selectedTags);
        this.$emit('filtered-posts', this.filteredPosts);
        // Aggiungi qui la logica per inviare i filtri al backend, se necessario
      }
    }
  };
  </script>

<template>
    <div class="container">
      <h1>Lista dei Post filtrati per Categoria e Tag</h1>
  
      <div class="mb-3">
        <h3>Filtra per Categoria:</h3>
        <div class="form-check" v-for="category in categories" :key="category.id">
          <input
            class="form-check-input"
            type="radio"
            :id="'category_' + category.id"
            :value="category.id"
            v-model="selectedCategory"
            @change="applyFilters"
          />
          <label class="form-check-label" :for="'category_' + category.id">{{ category.name }}</label>
        </div>
      </div>
  
      <div class="mb-3">
        <h3>Filtra per Tag:</h3>
        <div class="form-check" v-for="tag in tags" :key="tag.id">
          <input
            class="form-check-input"
            type="checkbox"
            :id="'tag_' + tag.id"
            :value="tag.id"
            v-model="selectedTags"
            @change="applyFilters"
          />
          <label class="form-check-label" :for="'tag_' + tag.id">{{ tag.name }}</label>
        </div>
      </div>
  
      <div v-for="(post, index) in filteredPosts" :key="post.id">
        <!-- Renderizzazione dei post filtrati -->
      </div>
  
      <button class="btn btn-primary" @click="submitFilters">Applica Filtri</button>
    </div>
  </template>
  
  
  
  <style scoped>
  /* Stili specifici per il componente */
  </style>
  