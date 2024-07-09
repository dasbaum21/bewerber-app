<template>
  <table>
    <thead>
    <tr>
      <th>Name</th>
      <th>Status</th>
      <th>Aktion</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="applicant in applicants" :key="applicant.id">
      <td>{{ applicant.name }}</td>
      <td>{{ getStatusText(applicant.status) }}</td>
      <td v-if="applicant.status === 'OPEN'">
        <button @click="$emit('accept-applicant', applicant)">Annehmen</button>
        <button @click="$emit('decline-applicant', applicant)">Ablehnen</button>
      </td>
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    applicants: {
      type: Array,
      required: true,
    },
  },
  methods: {
    getStatusText(status) {
      switch (status) {
        case 'OPEN':
          return 'Offen';
        case 'ACCEPTED':
          return 'Angenommen';
        case 'DECLINED':
          return 'Abgelehnt';
        default:
          return status;
      }
    }
  }
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

table, th, td {
  border: 1px solid #ddd;
}

th, td {
  padding: 12px;
  text-align: left;
}

th {
  background-color: #f4f4f4;
}

tr:nth-child(even) {
  background-color: #f9f9f9;
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
