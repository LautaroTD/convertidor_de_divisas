<template>
    <main class="app">
        <div class="container">
            <LlamadaApi />
            <TextoGuia />
        </div>
    </main>
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
    };
</script>

<style scoped>
    .app {
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        background: linear-gradient(135deg, #f0f4ff 0%, #eaf7f1 100%);
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial;
        padding: 32px;
        box-sizing: border-box;
    }

    .container {
        width: 100%;
        max-width: 980px;
        display: grid;
        grid-template-columns: 1fr 360px;
        gap: 24px;
        align-items: start;
    }

    /* Responsive */
    @media (max-width: 880px) {
        .container {
            grid-template-columns: 1fr;
            padding: 0 12px;
        }
    }
</style>