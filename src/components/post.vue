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
        .then(async response => {
            console.log(response);
            if (Array.isArray(response.data.data)) {
                this.posts = response.data.data;

                // Ottieni i dettagli dell'utente per ogni post
                for (let post of this.posts) {
                    await this.getUserDetails(post.userId);
                }
            } else {
                console.error('Errore durante la chiamata API: i dati non sono un array.');
            }
        })
        .catch(error => {
            console.error('Errore durante la chiamata API:', error);
        });
},
methods: {
    async getUserDetails(userId) {
    try {
        const userResponse = await axios.get(`http://localhost:3000/api/users/${userId}`);
        const user = userResponse.data;
        const postToUpdate = this.posts.find(post => post.userId === userId);
        if (postToUpdate) {
            postToUpdate.username = user.username;
        }
    } catch (error) {
        console.error(`Errore durante il recupero dell'utente con ID ${userId}:`, error);
    }
},

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

        <div v-for="(post, index) in posts" :key="post.id">

            <div class="card mb-3">
                <div class="card-header">
                    <h5>
                        Pubblicato da: {{ post.username }}
                    </h5>

                </div>

                <h4 class="card-title p-2">{{ post.title }}</h4>
                <img :src="post.image" class="card-img-top object-fit-cover " alt="Immagine post">
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
img{
    height: 300px;
}
</style>