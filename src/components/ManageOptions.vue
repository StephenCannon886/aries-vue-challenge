<template>
  <div>
    <div v-for="(option, index) in this.optionsData" :key="index" class="option-input">
      <label>Option {{ index + 1 }}</label>
      <select v-model="option.type">
      <option value="Call">Call</option>
      <option value="Put">Put</option>
      </select>
      <input type="number" v-model.number="option.strike_price" placeholder="Strike Price" required />
      <input type="number" v-model.number="option.ask" placeholder="Ask" required />
      <input type="number" v-model.number="option.bid" placeholder="Bid" required />
      <select v-model="option.long_short">
      <option value="long">Long</option>
      <option value="short">Short</option>
      </select>
      <button type="button" class="remove-button" @click="removeOption(index)">Remove</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ManageOptions',
  props: {
    optionsData: {
    type: Array,
    required: true
    }
  },
  methods: {
    removeOption(index) {
      this.$emit('remove-option', index)
    }
  }
}
</script>

<style scoped>
.option-input {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 5px;
  margin-bottom: 5px;
}

.option-input label {
  min-width: 80px;
}

input[type="number"], select {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

input[type="number"]:focus, select:focus {
  outline: none;
  border-color: dodgerblue;
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

.remove-button {
  background-color: rgb(240, 57, 29);
  margin-right: 10px;
}
</style>