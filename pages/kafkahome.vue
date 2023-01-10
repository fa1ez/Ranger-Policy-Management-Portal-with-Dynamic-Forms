<template>
  <v-container>
    <h1>KAFKA policies</h1>
    <router-link to="/kafka" style="text-decoration: none"
      ><v-btn color="sky blue" large class="mt-10 ml-10"
        ><v-icon left>mdi-plus-thick</v-icon>Create a new policy</v-btn
      ></router-link
    >
    <v-card v-for="policy in policies" :key="policy.id" flat class="mt-3 ml-10 mr-15">
      <v-layout wrap :class="`${policy.status}`" class="pa-3">
        <v-flex xs6 md3>
          <div class="caption grey--text">Name</div>
          <div>{{ policy.d_name }}</div>
        </v-flex>
        <v-flex md3>
          <div class="caption grey--text">Configerations</div>
          <div>{{ policy.connect }}</div>
        </v-flex>
        <v-flex md3>
          <div class="caption grey--text">Status</div>
          <div v-if="policy.status == true">active</div>
          <div v-else>idle</div>
        </v-flex>
        <v-flex md2>
          <div class="caption grey--text">Tag service</div>
          <div>{{ policy.tag_service }}</div>
        </v-flex>
        <v-flex xs1 md1 class="pt-2" justify-end>
          <v-btn small icon :to="{ name: 'kafka', params: { policy } }"
            ><v-icon dark>mdi-pencil</v-icon></v-btn
          >
          <v-btn small icon @click="del(policy.f_id)"
            ><v-icon dark>mdi-close-circle</v-icon></v-btn
          >
        </v-flex>
      </v-layout>
      <v-divider></v-divider>
    </v-card>
  </v-container>
</template>

<script>
import { getFirestore } from 'firebase/firestore'
import { collection, query, doc, getDocs, deleteDoc } from 'firebase/firestore'
export default {
  name: 'KafkaHomePage',
  data() {
    return {
      policies: {},
    }
  },
  created() {
    this.getData()
  },
  methods: {
    async del(pol) {
      const db = getFirestore()
      await deleteDoc(doc(db, 'kafka', pol))
      this.getData()
    },
    async getData() {
      const db = getFirestore()
      const q = query(collection(db, 'kafka'))
      const temp = {}
      const querySnapshot = await getDocs(q)
      querySnapshot.forEach((doc) => {
        temp[doc.id] = doc.data()
        temp[doc.id].f_id = doc.id
      })
      this.policies = temp
    },
  },
}
</script>

<style>
.true {
  border-left: 4px solid green;
}
.false {
  border-left: 4px solid red;
}
</style>
