<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" :clipped="clipped" app>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <img
        class="mx-5"
        :src="
          require('/Users/faiez.malik/Desktop/securiti_interns_summer_2022/apache_policy_portal/assets/logo2.png')
        "
        height="40"
      />
      <v-toolbar-title v-text="title" />
      <v-spacer />
    </v-app-bar>
    <v-main>
      <div class="container-wrapper">
        <v-container class="mx-5">
          <Nuxt />
        </v-container>
      </div>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: true,
      drawer: false,
      fixed: true,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-database',
          title: 'Hdfs',
          to: '/hdfshome',
        },
        {
          icon: 'mdi-apache-kafka',
          title: 'Kafka',
          to: '/kafkahome',
        },
        {
          icon: 'mdi-package-up',
          title: 'Hadoop Sql',
          to: '/hsqlhome',
        },
        {
          icon: 'mdi-elephant',
          title: 'Yarn',
          to: '/yarnhome',
        },
        {
          icon: 'mdi-dolphin',
          title: 'Hbase',
          to: '/hbasehome',
        },
        {
          icon: 'mdi-dots-triangle',
          title: 'Presto',
          to: '/prestohome',
        },
        {
          icon: 'mdi-form-select',
          title: 'All forms',
          to: '/All',
        },
      ],
      title: 'Apache Ranger Policy Management Portal',
    }
  },
}
</script>

<style lang="scss" scoped>
.container-wrapper {
  .container {
    max-width: inherit;
  }
}
</style>
