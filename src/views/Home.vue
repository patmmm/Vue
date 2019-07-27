<template>
  <v-content>
    <Toolbar />
    <v-container>

    </v-container>
  </v-content>
</template>

<script>
import Toolbar from '../components/Toolbar'
import Axios from 'axios'
export default {
  components: {
    Toolbar
  },
  data: () => ({
    persons: {}
  }),
  mounted: async function (params) {
    let response = await Axios.get('http://labkk.ga:3000/persons')
    this.persons = response.data
  }
}
</script>
