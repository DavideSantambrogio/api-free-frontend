<script>
import post from './post.vue'
import category from './category.vue'
import users from './users.vue'

export default {
    components: {
        post,
        category,
        users
    }

};

</script>

<template>
    <div class="row">
        <div class="col-3">
            <category @filtered-posts="updatePosts" />
        </div>

        <div class="col-6">
            <post :posts="filteredPosts" />
        </div>

        <div class="col-3">
            <users />
        </div>

    </div>


</template>


<style scoped>
/* Stili specifici per il componente */


</style>