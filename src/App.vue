<script>
import Navbar from './components/navbar.vue'
import vista from './components/vista.vue';


export default {
    components: {
        Navbar,
        vista
    }

}
</script>

<template>
<Navbar />
<div>
    <vista />
</div>

</template>

<style lang="scss">
@use "./style/general.scss";
@import "@fortawesome/fontawesome-free/css/all.css"; 
*{
    // border: 1px solid red;
}

</style>