<template>
    <div>
        <h3>Converidor de Divisa con ExchangeRate API</h3>

        <button @click="convertir">Convertir</button>

        <div>
            <input v-model="origen" type="text" placeholder="Ingrese moneda origen" />
            <input v-model="objetivo" type="text" placeholder="Ingrese moneda objetivo" />

            <input v-model="monto" type="number" placeholder="Ingrese monto" />
            <p>resultado: {{ monto * rate }}</p>

        </div>

        <p v-if="rate">Tasa actual: {{ rate }}</p>
        <p v-if="error">Error: {{ error }}</p>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                rate: null,
                error: null
            };
        },

        methods: {
            async convertir() {
                const API_KEY = "d3e1cd3cf3a237c71d91eec9";

                try {
                    const res = await fetch(
                        `https://v6.exchangerate-api.com/v6/${API_KEY}/pair/${this.origen}/${this.objetivo}`
                    );
                    const data = await res.json();

                    if (data.result === "success") {
                        this.rate = data.conversion_rate;
                        this.error = null;
                    } else {
                        this.error = "Error en la API";
                    }
                } catch (e) {
                    this.error = e.message;
                }
            }
        }
    };
</script>