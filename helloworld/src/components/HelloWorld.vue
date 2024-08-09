<template>
  <div class="dog-breeds" id="dogBreeds">
    <select v-model="selectedBreed" @change="onBreedSelected">
      <option disabled value="">Select a breed</option>
      <option v-for="breed in breeds" :key="breed.id" :value="breed.name">
        {{ breed.name }}
      </option>
    </select>
    <p id="fieldValue">Selected Breed: {{ selectedBreed || 'No breed selected' }}</p>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script>
import UiExtension from "@bloomreach/ui-extension-saas";

export default {
  name: 'DogBreeds',
  data() {
    return {
      breeds: [],
      selectedBreed: '',
      error: null,
      ui: null,
    };
  },
  mounted() {
    this.fetchBreeds();
    this.initializeUiExtension();
  },
  methods: {
    fetchBreeds() {
      fetch('https://api.thedogapi.com/v1/breeds')
          .then(response => {
            if (!response.ok) {
              throw new Error(`HTTP error! Status: ${response.status}`);
            }
            return response.json();
          })
          .then(data => {
            this.breeds = data;
          })
          .catch(error => {
            this.error = `Error fetching dog breeds: ${error.message}`;
            console.error(this.error);
          });
    },
    async initializeUiExtension() {
      document.addEventListener('DOMContentLoaded', async () => {
        try {
          this.ui = await UiExtension.register();
          const value = await this.ui.document.field.getValue();
          this.selectedBreed = value || '';  // Set default value if any
        } catch (error) {
          console.error('Failed to register extension:', error.message);
          console.error('- error code:', error.code);
        }
      });
    },
    async onBreedSelected() {
      try {
        if (this.ui) {
          await this.ui.document.field.setValue(this.selectedBreed);
          this.showFieldValue(this.selectedBreed);
        }
      } catch (error) {
        console.error('Error setting field value: ', error.code, error.message);
      }
    },
    showFieldValue(value) {
      document.querySelector('#fieldValue').innerHTML = `Selected Breed: ${value}`;
    }
  }
}
</script>

<style>
.dog-breeds {
  margin: 20px;
  height: 100px;
}

#dogBreeds {
  height: 50px;
  text-align: center;
}

select {
  padding: 5px;
  font-size: 16px;
}
</style>
