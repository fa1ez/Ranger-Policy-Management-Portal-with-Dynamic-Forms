<template>
  <div class="mr-10">
    <h1 class="text-h4 font-weight-thin mb-4">
      Welcome To The Service Manager!
    </h1> 
    <hr />
    <br />
    <v-img
      :src="
        require('/Users/faiez.malik/Desktop/securiti_interns_summer_2022/apache_policy_portal/assets/apache2.png')
      "
      max-height="200"
    ></v-img>
    <center>
      <h3 class="subheading py-3">Make policies for the following services!</h3>
      <v-layout row wrap justify-space-around class="ma-5">
        <v-flex xs12 s6 md4 class="pb-3">
          <router-link
            to="/hdfshome"
            style="text-decoration: none; color: inherit"
            ><div class="box"><h1>HDFS</h1></div></router-link
          >
        </v-flex>
        <v-flex xs12 s6 md4 class="pb-3">
          <router-link
            to="/kafkahome"
            style="text-decoration: none; color: inherit"
            ><div class="box"><h1>KAFKA</h1></div></router-link
          >
        </v-flex>
        <v-flex xs12 s6 md4 class="pb-3">
          <router-link
            to="/hsqlhome"
            style="text-decoration: none; color: inherit"
            ><div class="box"><h1>Hadoop SQL</h1></div></router-link
          >
        </v-flex>
      </v-layout>
      <v-layout row wrap justify-space-around class="ma-5 pt-5">
        <v-flex xs12 s6 md4 class="pb-3">
          <router-link
            to="/yarnhome"
            style="text-decoration: none; color: inherit"
            ><div class="box"><h1>YARN</h1></div></router-link
          >
        </v-flex>
        <v-flex xs12 s6 md4 class="pb-3">
          <router-link
            to="/hbasehome"
            style="text-decoration: none; color: inherit"
            ><div class="box"><h1>HBASE</h1></div></router-link
          >
        </v-flex>
        <v-flex xs12 s6 md4 class="pb-3">
          <router-link
            to="/prestohome"
            style="text-decoration: none; color: inherit"
            ><div class="box"><h1>PRESTO</h1></div></router-link
          >
        </v-flex>
      </v-layout>
    </center>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {}
  },
}
</script>

<style scoped>
.box {
  width: 250px;
  height: 150px;
  background: rgb(0, 103, 158);
  transition: 1s;
  text-align: center;
  padding-top: 50px;
  text-decoration: none;
}
.box:hover {
  transition: 0.6s;
  transform: scale(1.3);
  background: #73b8f1;
  box-shadow: 2px 2px 2px 2px #000;
}
</style>
