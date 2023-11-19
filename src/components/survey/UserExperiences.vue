<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <ul v-if="!isLoading">
        <survey-result v-for="result in results" :key="result.id" :name="result.name"
          :rating="result.rating"></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import axios from 'axios';
import Swal from 'sweetalert2'
import SurveyResult from './SurveyResult.vue';

export default {
  data() {
    return {
      results: [],
      isLoading: false,
    }
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      axios.get('https://satisfaction-form-258b6-default-rtdb.firebaseio.com/surveys.json')
        .then((response) => {
          this.isLoading = false;
          const results = [];
          for (const id in response.data) {
            results.push({ id: id, name: response.data[id].name, rating: response.data[id].rating });
          }
          this.results = results;
        })
    }

  },
  components: {
    SurveyResult,
  },
  mounted() {
    this.loadExperiences();
  },
  watch: {
    isLoading(newValue) {
      if (newValue) {
        Swal.fire({
          title: "Loading",
          html: "Loading the submited experiences",
          didOpen: () => {
            Swal.showLoading();
          },
        });
      } else {
        Swal.close();
      }
    }
  }

};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>