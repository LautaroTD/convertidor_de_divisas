<template>
    <aside class="side-card">
        <h4>Códigos de moneda</h4>
        <div class="chip-list" title="Haz clic para copiar">
            <button v-for="code in currencies"
                    :key="code"
                    class="chip"
                    @click="copy(code)"
                    :aria-label="`Copiar ${code}`">
                {{ code }}
            </button>
        </div>
        <p class="hint">Haz clic en un código para copiarlo al portapapeles.</p>
    </aside>
</template>

<script>
    export default {
        name: 'TextoGuia',
        data() {
            return {
                currencies: [
                    'USD', 'AED', 'AFN', 'ALL', 'AMD', 'ANG', 'AOA', 'ARS', 'AUD', 'AWG', 'AZN', 'BAM', 'BBD', 'BDT', 'BGN', 'BHD', 'BIF', 'BMD', 'BND', 'BOB', 'BRL', 'BSD', 'BTN', 'BWP', 'BYN', 'BZD', 'CAD', 'CDF', 'CHF', 'CLF', 'CLP', 'CNH', 'CNY', 'COP', 'CRC', 'CUP', 'CVE', 'CZK', 'DJF', 'DKK', 'DOP', 'DZD', 'EGP', 'ERN', 'ETB', 'EUR', 'FJD', 'FKP', 'FOK', 'GBP', 'GEL', 'GGP', 'GHS', 'GIP', 'GMD', 'GNF', 'GTQ', 'GYD', 'HKD', 'HNL', 'HRK', 'HTG', 'HUF', 'IDR', 'ILS', 'IMP', 'INR', 'IQD', 'IRR', 'ISK', 'JEP', 'JMD', 'JOD', 'JPY', 'KES', 'KGS', 'KHR', 'KID', 'KMF', 'KRW', 'KWD', 'KYD', 'KZT', 'LAK', 'LBP', 'LKR', 'LRD', 'LSL', 'LYD', 'MAD', 'MDL', 'MGA', 'MKD', 'MMK', 'MNT', 'MOP', 'MRU', 'MUR', 'MVR', 'MWK', 'MXN', 'MYR', 'MZN', 'NAD', 'NGN', 'NIO', 'NOK', 'NPR', 'NZD', 'OMR', 'PAB', 'PEN', 'PGK', 'PHP', 'PKR', 'PLN', 'PYG', 'QAR', 'RON', 'RSD', 'RUB', 'RWF', 'SAR', 'SBD', 'SCR', 'SDG', 'SEK', 'SGD', 'SHP', 'SLE', 'SLL', 'SOS', 'SRD', 'SSP', 'STN', 'SYP', 'SZL', 'THB', 'TJS', 'TMT', 'TND', 'TOP', 'TRY', 'TTD', 'TVD', 'TWD', 'TZS', 'UAH', 'UGX', 'UYU', 'UZS', 'VES', 'VND', 'VUV', 'WST', 'XAF', 'XCD', 'XCG', 'XDR', 'XOF', 'XPF', 'YER', 'ZAR', 'ZMW', 'ZWL'
                ]
            };
        },
        methods: {
            async copy(code) {
                try {
                    await navigator.clipboard.writeText(code);
                    // breve feedback visual
                    const el = event.currentTarget;
                    const original = el.innerText;
                    el.innerText = 'Copiado ✓';
                    setTimeout(() => (el.innerText = original), 800);
                } catch {
                    alert('No se pudo copiar. Usa Ctrl+C.');
                }
            }
        }
    };
</script>

<style scoped>
    .side-card {
        background: linear-gradient(180deg, #ffffff 0%, #fbfdff 100%);
        border-radius: 12px;
        padding: 14px;
        box-shadow: 0 6px 20px rgba(12, 20, 40, 0.06);
        max-height: 560px;
        overflow: hidden;
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

        .side-card h4 {
            margin: 0;
            color: #0f172a;
            font-size: 1rem;
        }

    .chip-list {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
        overflow: auto;
        padding: 6px 2px;
        max-height: 420px;
    }

    .chip {
        background: #f1f8ff;
        border: 1px solid #e3f2ff;
        color: #0b3a66;
        padding: 6px 10px;
        border-radius: 999px;
        font-weight: 600;
        cursor: pointer;
        transition: transform .08s, background .12s;
    }

        .chip:hover {
            transform: translateY(-2px);
            background: #dbeeff;
        }

    .hint {
        margin: 0;
        color: #64748b;
        font-size: 0.8rem;
    }
</style>