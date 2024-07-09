<template>
  <div id="app">
    <ApplicantForm @add-applicant="addApplicant" />
    <div>
      <input v-model="nameFilter" placeholder="Namensfilter" />
      <label>
        <input type="checkbox" v-model="hideDeclined" />
        Abgelehnte Bewerber ausblenden
      </label>
    </div>
    <ApplicantTable
        :applicants="filteredApplicants"
        @accept-applicant="acceptApplicant"
        @decline-applicant="declineApplicant"
    />
  </div>
</template>

<script>
import ApplicantForm from './components/ApplicantForm.vue';
import ApplicantTable from './components/ApplicantTable.vue';

export default {
  components: {
    ApplicantForm,
    ApplicantTable
  },
  data() {
    return {
      nameFilter: '',
      hideDeclined: false,
      applicants: []
    };
  },
  computed: {
    filteredApplicants() {
      return this.applicants.filter((applicant) => {
        const matchesNameFilter = applicant.name.toLowerCase().includes(this.nameFilter.toLowerCase());
        const matchesStatusFilter = this.hideDeclined ? applicant.status !== 'DECLINED' : true;
        return matchesNameFilter && matchesStatusFilter;
      });
    },
  },
  methods: {
    async fetchApplicants() {
      try {
        const response = await this.$axios.get('/applicant');
        this.applicants = response.data;
      } catch (error) {
        console.error('Error fetching applicants:', error);
      }
    },
    async addApplicant(name) {
      if (name) {
        try {
          const response = await this.$axios.post('/applicant', { name });
          this.applicants.push(response.data);
        } catch (error) {
          console.error('Error adding applicant:', error);
        }
      }
    },
    async acceptApplicant(applicant) {
      try {
        applicant.status = 'ACCEPTED';
        await this.$axios.put(`/applicant/${applicant.id}`, applicant);
      } catch (error) {
        console.error('Error accepting applicant:', error.response ? error.response.data : error.message);
      }
    },

    async declineApplicant(applicant) {
      try {
        applicant.status = 'DECLINED';
        await this.$axios.put(`/applicant/${applicant.id}`, applicant);
      } catch (error) {
        console.error('Error declining applicant:', error.response ? error.response.data : error.message);
      }
    }

  },
  mounted() {
    this.fetchApplicants();
  }
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
  padding: 20px;
}

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

label {
  display: inline-flex;
  align-items: center;
  margin: 10px;
}

input[type="checkbox"] {
  margin-right: 5px;
}
</style>
