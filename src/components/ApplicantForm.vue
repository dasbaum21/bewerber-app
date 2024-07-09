<template>
  <div>
    <input v-model="name" placeholder="Bewerbername eingeben" />
    <button @click="handleAddApplicant">Hinzufügen</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
    };
  },
  methods: {
    handleAddApplicant() {
      this.$emit('add-applicant', this.name);
      this.name = '';
    },
  },
};
</script>

<style scoped>
input[type="text"] {
  padding: 10px;
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 20px;
  margin: 10px;
  background-color: #42b983;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #38a169;
}
</style>
