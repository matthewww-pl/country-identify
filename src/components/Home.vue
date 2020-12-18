<template>
  <div>
    <b-container >
      <b-jumbotron>
        <template #header>Country Identify App</template>
        <template #lead>
          Application to display information about countries from around the world. Check it out.
        </template>
        <hr class="my-4">
        <b-row>
          <b-col sm="12" md="6">
            <b-form @submit="onSubmit" @reset="onReset" v-if="show">
              Search your country on bottom <b-icon icon="arrow-down" animation="cylon-vertical" font-scale="2"></b-icon>
              <b-form-group
                id="input-group-1"
                label-for="input-1"
                description="Allow uppercase and lowercase"
                class="mt-2"
              >
                <b-form-input
                  id="input-1"
                  v-model="form.name"
                  type="text"
                  placeholder="Enter Country"
                  required
                ></b-form-input>
              </b-form-group>
              <b-button type="submit" variant="success">Search</b-button>
              <b-button class="ml-2" type="reset" variant="danger">Reset</b-button>
            </b-form>
          </b-col>
        </b-row>
        <hr class="my-4">
        <b-row v-if="data.length!==0">
          <b-col sm="12" md="6">
            <b-table striped hover :items="data"></b-table>
          </b-col>
          <b-col sm="12" md="6">
            <h5>Flag</h5>
            <b-img class="mt-2" :src="flag" fluid alt="Flag"/>
          </b-col>
        </b-row>
      </b-jumbotron>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      form: {
        name: ''
      },
      show: true,
      fields: ['Param', 'Info'],
      data: [],
      flag: null,
    }
  },
  methods: {
    onSubmit(event) {
      event.preventDefault()
      this.data = []
      this.flag = null
      axios.get('https://restcountries.eu/rest/v2/name/'+this.form.name).then(response => {
        this.flag = response.data[0].flag
        this.data.push({'param': 'Name', 'info': response.data[0].name})
        this.data.push({'param': 'Capital', 'info': response.data[0].capital})
        this.data.push({'param': 'Language', 'info': response.data[0].languages[0].name})
        this.data.push({'param': 'Alpha 2 Code', 'info': response.data[0].alpha2Code})
        this.data.push({'param': 'Alpha 3 Code', 'info': response.data[0].alpha3Code})
        this.data.push({'param': 'Population', 'info': response.data[0].population})
        this.data.push({'param': 'Calling Code', 'info': response.data[0].callingCodes[0]})
        this.data.push({'param': 'Region', 'info': response.data[0].region})
      }).catch(error => {
        if(error.response.status===404){
          this.makeToast('b-toaster-top-center','danger')
        }
      })
    },
    onReset(event) {
      event.preventDefault()
      this.form.name = ''
      this.data = []
      this.flag = null
    },
    makeToast(toaster, variant = null) {
      this.$bvToast.toast('Please enter a country name', {
        title: `Error`,
        toaster: toaster,
        variant: variant,
        solid: true
      })
    }
  }
}
</script>

<style>
  body{
    background-color: #292222;
  }
</style>