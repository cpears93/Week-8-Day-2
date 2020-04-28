<template lang="html">
  <div>
    <div>
    <input type="number" name="Amount" v-model.number="amount">
  </div>
  <div>
    <form v-if="amount">
      <select v-on:change="handleSelectFrom"v-model="from">
        <option disabledValue="">Select a Currency</option>
        <option v-for="(currency, index) in currencies":value="currency"> {{currency.name}}
        </option>
      </select>
    </form>
    <form v-if="amount">
      <select v-on:change="handleSelectTo"v-model="to">
        <option disabledValue="">Select a Currency</option>
        <option v-for="(currency, index) in currencies":value="currency"> {{currency.name}}
        </option>
      </select>
    </form>
    <div v-if="from&&to">
      {{conversion}}
    </div>
  </div>

  </div>

</template>

<script>
export default {
  name: "App",
  data(){
    return {
      currencies: [],
      from: null,
      to: null,
      amount: null,
      conversion: 0
  }},
  mounted(){
    fetch("https://api.exchangeratesapi.io/latest")
    .then(result => result.json())//.then(data => this.currencies = data.rates
    .then(data => {
      for (let key of Object.keys(data.rates)) {
        console.log(key);
      const newCurrency = {name: key,
                    value: data.rates[key]}
      this.currencies.push(newCurrency)}
    })
  },
  computed: {
    converting: function(){
      this.conversion = (this.from.value*this.amount)/this.to.value
    }
  },
  watch: {
    conversion: function(){
      this.converting()
    }
  }
}

</script>

<style lang="css" scoped>
</style>
