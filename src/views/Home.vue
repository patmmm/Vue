<template>
  <v-content>
    <Toolbar />
    <v-container grid-list-xl>
      <v-layout column wrap>
        <v-flex>
          <PeopleTable />
        </v-flex>
        <v-flex>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script>
import PeopleTable from '../components/PeopleTable.vue'
import Toolbar from '../components/Toolbar'

export default {
  components: {
    PeopleTable,
    Toolbar
  },
  data: () => ({
    showDialogMsg: true
  })
}
</script>
