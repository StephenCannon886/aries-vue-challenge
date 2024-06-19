<template>
  <div class="container">
    <h1>Options Profit Calculator</h1>
    <ManageOptions :optionsData="options" @remove-option="removeOption"/>
    <button class="generate-button" @click="generateGraph">Generate Graph</button>
    <ApexChart v-if="showGraph" :series="series" :maxProfit="maxProfit" :maxLoss="maxLoss" :breakEvenPoints="breakEvenPoints"></ApexChart>
  </div>
</template>

<script>
import ManageOptions from './ManageOptions.vue'
import ApexChart from './ApexChart.vue'

export default {
  name: 'CodingChallenge',
  components: {
    ManageOptions,
    ApexChart
  },
  props: {
    optionsData: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      showGraph: false,
      options: this.optionsData,
      series: [{
        name: 'Profit/Loss',
        data: []
      }],
      maxProfit: null,
      maxLoss: null,
      breakEvenPoints: []
    }
  },
  methods: {
    removeOption(index) {
      this.options.splice(index, 1)
    },
    generateGraph() {
      const priceRange = this.generatePriceRange()
      const profitLossData = priceRange.map(price => this.calculateProfitLoss(price))
      this.series = [{
        name: 'Profit/Loss',
        data: priceRange.map((_, index) => ({ x: priceRange[index], y: profitLossData[index] }))
      }]
      this.calculateMaxMin(priceRange, profitLossData)
      this.showGraph = true
    },
    generatePriceRange() {
      const minStrike = Math.min(...this.options.map(option => option.strike_price))
      const maxStrike = Math.max(...this.options.map(option => option.strike_price))

      const delta = 50
      const startStrike = Math.floor((minStrike - delta) / delta) * delta
      const endStrike = Math.ceil((maxStrike + delta) / delta) * delta

      const prices = []
      for (let i = startStrike; i <= endStrike; i++)
        prices.push(i)
      return prices
    },
    calculateProfitLoss(price) {
      return this.options.reduce((total, option) => {
        const { strike_price, type, bid, ask, long_short } = option  
        const premium = (bid + ask) / 2  
        let payoff = 0  

        if (type === 'Call')
          payoff = Math.max(price - strike_price, 0)  
        else if (type === 'Put')
          payoff = Math.max(strike_price - price, 0)  

        if (long_short === 'long')
          return total + (payoff - premium)  
        else
          return total + (premium - payoff)  
      }, 0)
    },
    calculateMaxMin(priceRange, profitLossData) {
      this.maxProfit = Math.max(...profitLossData)
      this.maxLoss = Math.min(...profitLossData)
      this.breakEvenPoints = this.findBreakEvenPoints(priceRange, profitLossData).map(price => { 
        return { x: price, y: profitLossData[priceRange.indexOf(price)] }
      })
    },
    findBreakEvenPoints(priceRange, profitLossData) {
      return priceRange.filter((price, index) => {
        return (profitLossData[index - 1] < 0 && profitLossData[index] > 0) || (profitLossData[index - 1] > 0 && profitLossData[index] < 0)  
      })  
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 1000px;
  margin: auto;
  padding: 10px;
  font-family: 'Arial', sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

button {
  padding: 10px 20px;
  background-color: dodgerblue;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #007acc;
}

.generate-button {
  background-color: dodgerblue;
}
</style>