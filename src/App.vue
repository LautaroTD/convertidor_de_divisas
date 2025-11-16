<template>
    <LlamadaApi />
    <TextoGuia/>
</template>


<script>
    import LlamadaApi from './components/LlamadaApi.vue';
    import TextoGuia from './components/TextoGuia.vue';

    export default {
        name: 'App',
        components: {
            LlamadaApi,
            TextoGuia
        }
    }
</script>


<style>

</style>
