<script>
import axios from 'axios';

export default {
    data() {
        return {
            posts: []
        };
    },
    mounted() {
        axios.get('http://localhost:3000/api/posts')
            .then(response => {
                console.log(response);
                this.posts = response.data;
                // console.log(this.posts);
            })
            .catch(error => {
                console.error('Errore durante la chiamata API:', error);

            });

    },
    methods: {
        formatDate(dateString) {
            const options = { year: 'numeric', month: 'long', day: 'numeric' };
            return new Date(dateString).toLocaleDateString('it-IT', options);
        }
    }
};
</script>

<template>
    <div class="container">
        <h1>Lista dei Post</h1>

        <div v-for="(post, index) in posts.data" :key="post.id">

            <div class="card mb-3">
                <div class="card-header">
                    <h5>
                        Pubblicato da: {{ post.username }}
                    </h5>
                    
                </div>

                <h4 class="card-title p-2">{{ post.title }}</h4>
                <img :src="post.image" class="card-img-top" alt="Immagine post">
                <div class="card-body">

                    <p class="card-text">{{ post.content }}</p>
                    <ul class="list-group list-group-flush">
                        <li class="list-group-item">Categoria: {{ post.category.name }}</li>
                        <li class="list-group-item">Tags:
                            <span v-for="tag in post.tags" :key="tag.id" class="badge bg-secondary">{{ tag.name
                                }}</span>
                        </li>
                    </ul>
                </div>
                <div class="card-footer">
                    <small class="text-muted">Pubblicato il: {{ formatDate(post.createdAt) }}</small>
                </div>


            </div>
        </div>
    </div>
</template>



<style scoped>
/* Stili specifici per il componente */
</style>