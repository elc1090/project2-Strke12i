<template>
    <div class="relative pl-2">
      <input
        type="text"
        class="h-10 pl-2 pr-5 text-gray-800 border border-gray-300 rounded-lg focus:outline-none focus:border-indigo-500"
        placeholder="Busca sua banda aqui..."
        v-model="inputValue" 
        @input="emitSearchInput"
      />
    </div>
  </template>
  
  <script>
export default {
  data() {
    return {
      inputValue: ''
    };
  },
  methods: {
    emitSearchInput() {
      this.$emit('search-input', this.inputValue)
    },
  },
};
</script>
  
  <style scoped>
  input[type="text"]::-webkit-search-cancel-button,
  input[type="text"]::-webkit-search-decoration,
  input[type="text"]::-webkit-search-results-button,
  input[type="text"]::-webkit-search-results-decoration {
    display: none;
  }
  </style>
  