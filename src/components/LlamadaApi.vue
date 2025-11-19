<template>
    <section class="card">
        <header class="card-header">
            <h1>Convertidor de Divisas</h1>
            <small>Usa ExchangeRate API</small>
        </header>

        <div class="card-body">
            <div class="form-row">
                <input v-model="origen" type="text" placeholder="Moneda origen (ej. USD)" maxlength="3" />
                <input v-model="objetivo" type="text" placeholder="Moneda objetivo (ej. EUR)" maxlength="3" />
            </div>

            <div class="form-row">
                <input v-model.number="monto" type="number" min="0" step="any" placeholder="Monto" />
                <button @click="convertir" :disabled="loading || !valid" class="btn">
                    {{ loading ? 'Convirtiendo…' : 'Convertir' }}
                </button>
            </div>

            <div class="result">
                <p v-if="rate" class="success">
                    Resultado: <strong>{{ formattedResult }}</strong>
                </p>
                <p v-if="rate" class="meta">Tasa: {{ rate }}</p>
                <p v-if="error" class="error">Error: {{ error }}</p>
            </div>
        </div>

        <footer class="card-footer">
            <small>Consejo: usa codigos ISO 4217 (3 letras). Mueve el raton sobre la lista de la derecha para copiar codigos.</small>
        </footer>
    </section>
</template>

<script>
    export default {
        name: 'LlamadaApi',
        data() {
            return {
                origen: 'USD',
                objetivo: 'EUR',
                monto: 1,
                rate: null,
                error: null,
                loading: false
            };
        },
        computed: {
            valid() {
                return (
                    this.origen &&
                    this.objetivo &&
                    this.origen.trim().length === 3 &&
                    this.objetivo.trim().length === 3 &&
                    Number(this.monto) >= 0
                );
            },
            formattedResult() {
                if (!this.rate) return '';
                const value = (Number(this.monto) * Number(this.rate));
                return `${value.toFixed(4)} ${this.objetivo.toUpperCase()}`;
            }
        },
        methods: {
            async convertir() {
                if (!this.valid) {
                    this.error = 'Verifica los códigos (3 letras) y el monto.';
                    this.rate = null;
                    return;
                }

                this.loading = true;
                this.error = null;
                this.rate = null;

                const API_KEY = 'd3e1cd3cf3a237c71d91eec9';

                try {
                    const from = this.origen.trim().toUpperCase();
                    const to = this.objetivo.trim().toUpperCase();

                    const res = await fetch(
                        `https://v6.exchangerate-api.com/v6/${API_KEY}/pair/${from}/${to}`
                    );

                    if (!res.ok) {
                        throw new Error(`HTTP ${res.status}`);
                    }

                    const data = await res.json();

                    if (data.result === 'success' && data.conversion_rate != null) {
                        this.rate = data.conversion_rate;
                        this.error = null;
                    } else {
                        this.error = data['error-type'] || 'Respuesta inesperada de la API';
                    }
                } catch (e) {
                    this.error = e.message || 'Error desconocido';
                } finally {
                    this.loading = false;
                }
            }
        }
    };
</script>

<style scoped>
    .card {
        background: #ffffff;
        border-radius: 12px;
        box-shadow: 0 6px 24px rgba(16, 24, 40, 0.08);
        padding: 18px;
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .card-header h1 {
        margin: 0;
        font-size: 1.25rem;
        color: #0f172a;
    }

    .card-header small {
        color: #64748b;
    }

    .card-body {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .form-row {
        display: flex;
        gap: 8px;
    }

        .form-row input[type="text"],
        .form-row input[type="number"] {
            flex: 1;
            padding: 10px 12px;
            border-radius: 8px;
            border: 1px solid #e6eef8;
            background: #f8fbff;
            font-size: 0.95rem;
            outline: none;
            transition: box-shadow .12s, border-color .12s;
        }

        .form-row input:focus {
            border-color: #7c93f2;
            box-shadow: 0 4px 16px rgba(124,147,242,0.12);
        }

    .btn {
        padding: 10px 14px;
        border-radius: 8px;
        border: none;
        background: linear-gradient(90deg, #5b8cff, #6fe7c6);
        color: white;
        cursor: pointer;
        font-weight: 600;
        min-width: 120px;
    }

        .btn[disabled] {
            opacity: 0.6;
            cursor: default;
        }

    .result .success {
        color: #064e3b;
        font-size: 1rem;
    }

    .result .meta {
        color: #475569;
        font-size: 0.85rem;
    }

    .error {
        color: #b91c1c;
    }

    .card-footer small {
        color: #475569;
    }
</style>