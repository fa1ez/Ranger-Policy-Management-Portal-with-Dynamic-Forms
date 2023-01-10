<template>
  <v-container>
    <h1>Create new policy</h1>
    <br />
    <v-card color="white" light style="width: 70%;">
      <dynamic
        :dat="dats"
        :initialData="initialData"
        @retdata="store($event, $router)"
      />
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios'
import { getFirestore } from 'firebase/firestore'
import { collection, addDoc, doc, setDoc } from 'firebase/firestore'
import Dynamic from '../components/dynamic-form.vue'

export default {
  name: 'HsqlPage',
  components: { dynamic: Dynamic },
  data() {
    return {
      dats: [],
      formdata: {},
      initialData: {},
    }
  },
  created() {
    axios.get('http://localhost:3032/hsql_form').then((res) => {
      this.dats = res.data
    })
    this.initialData = this.$route.params
  },
  methods: {
    async store(ev, router) {
      this.formdata = ev
      const db = getFirestore()
      const temp = this.formdata
      if (this.formdata.f_id) {
        // delete temp["f_id"]
        // how to delete f_id from object
        await setDoc(doc(db, 'hsql', this.formdata.f_id), temp)
      } else {
        const docRef = await addDoc(collection(db, 'hsql'), this.formdata)
        console.log('Document written with ID: ', docRef.id)
      }
      router.push('hsqlhome')
    },
  },
}
</script>
