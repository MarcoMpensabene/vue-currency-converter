<script>
export default {
    props: ["currencies", "amount", "selectedCurrency", "selectedCurrencyOther"],
    data() {
        return {
            localAmount: this.amount,
            localCurrency: this.selectedCurrency,
        };
    },
    watch: {
        amount(newVal) { this.localAmount = newVal; },
        selectedCurrency(newVal) { this.localCurrency = newVal; },
    },
    methods: {
        updateAmount() { this.$emit("update-amount", this.localAmount); },
        updateCurrency() { this.$emit("update-currency", this.localCurrency); },
    },
};
</script>

<template>
    <div class="currency-input">
        <input type="number" v-model="localAmount" @input="updateAmount" placeholder="Inserisci importo" />
        <select v-model="localCurrency" @change="updateCurrency">
            <option v-for="currency in currencies" :key="currency" :value="currency"
                :disabled="currency === selectedCurrencyOther">
                {{ currency }}
            </option>
        </select>
    </div>
</template>

<style scoped>
.currency-input {
    display: flex;
    gap: 10px;
    margin-bottom: 10px;
}

input[type="number"] {
    padding: 8px;
    width: 100%;
    font-size: 1em;
    border-radius: 4px;
    border: 1px solid #ddd;
}

select {
    padding: 8px;
    font-size: 1em;
    border-radius: 4px;
    border: 1px solid #ddd;
}
</style>
