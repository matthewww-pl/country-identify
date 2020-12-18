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
          <b-col cols="6">
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
      </b-jumbotron>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        name: ''
      },
      show: true
    }
  },
  methods: {
    onSubmit(event) {
      event.preventDefault()
      alert(JSON.stringify(this.form))
    },
    onReset(event) {
      event.preventDefault()
      this.form.name = ''
    }
  }
}
</script>

<style>
  body{
    background-color: #292222;
  }
</style>