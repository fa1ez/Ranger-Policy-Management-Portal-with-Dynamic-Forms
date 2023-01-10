<template>
  <v-container>
    <v-card style="margin-right: 50px; background-color: darkslategrey;">
      <h1 style="" class="pa-5"><i>
        <!-- Examples Of Differet Types Of Inputs Supported By Dynamic Forms -->
        Simple Documentation
        </i>
      </h1>
    </v-card>
    <v-card dark style="margin-right: 50px">
      <dynamic-form :dat="dats" />
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios'
import Dynamic from '../components/dynamic-form.vue'
export default {
  name: 'AllPage',
  components: { 'dynamic-form': Dynamic },
  data() {
    return {
      dats: [],
      grp: [],
    }
  },
  created() {
    axios.get('http://localhost:3036/forms').then((res) => {
      this.dats = res.data
    })
  },
}
</script>
