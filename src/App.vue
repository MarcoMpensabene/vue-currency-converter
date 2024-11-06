<script>
import axios from "axios";
import CurrencyInput from "./components/CurrencyInput.vue";


export default {
  components: { CurrencyInput },
  data() {
    return {
      currencies: [],
      initialCurrency: "EUR",
      convertedCurrency: "USD",
      amountFrom: 1,
      amountTo: 0,
    };
  },
  async created() {
    try {
      const response = await axios.get("https://api.frankfurter.app/currencies");
      this.currencies = Object.keys(response.data);
    } catch (error) {
      console.error("Errore nel caricamento delle valute", error);
    }
    this.convert(); // Conversione iniziale
  },
  methods: {
    updateAmountFrom(amount) { this.amountFrom = amount; this.convert(); },
    updateCurrencyFrom(currency) { this.initialCurrency = currency; this.convert(); },
    updateAmountTo(amount) { this.amountTo = amount; },
    updateCurrencyTo(currency) { this.convertedCurrency = currency; this.convert(); },
    async convert() {
      if (this.initialCurrency !== this.convertedCurrency) {
        const response = await axios.get(`https://api.frankfurter.app/latest?amount=${this.amountFrom}&from=${this.initialCurrency}&to=${this.convertedCurrency}`);
        this.amountTo = response.data.rates[this.convertedCurrency].toFixed(2); // Mostra con due decimali
      } else {
        this.amountTo = this.amountFrom;
      }
    },
  },
};
</script>

<template>
  <div id="app" class="container">
    <h1>Convertitore di Valute</h1>


    <div class="conversion-result">
      <p>{{ amountFrom }} {{ currencyFrom }} è circa {{ amountTo }} {{ currencyTo }}</p>
    </div>

    <CurrencyInput :currencies="currencies" :amount="amountFrom" :selectedCurrency="initialCurrency"
      @update-amount="updateAmountFrom" @update-currency="updateCurrencyFrom"
      :selectedCurrencyOther="convertedCurrency" />

    <CurrencyInput :currencies="currencies" :amount="amountTo" :selectedCurrency="cconvertedCurrency"
      @update-amount="updateAmountTo" @update-currency="updateCurrencyTo" :selectedCurrencyOther="initialCurrency" />

  </div>
</template>

<style>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 2em;
  color: #333;
  margin-bottom: 20px;
}

.conversion-result {
  font-size: 1.2em;
  font-weight: bold;
  margin-bottom: 20px;
  color: #4caf50;
}
</style>
