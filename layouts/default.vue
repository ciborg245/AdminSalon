<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      clipped
      absolute
      temporary
      app
    >
      <v-list>
        <v-list-tile avatar>
          <v-list-tile-content>
            <v-list-tile-title>Menu</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    <v-divider></v-divider>
    <v-list>
      <v-list-tile
        router
        :to="item.to"
        :key="i"
        v-for="(item, i) in items"
        exact
      >
        <v-list-tile-action>
          <v-icon v-html="item.icon"></v-icon>
        </v-list-tile-action>
        <v-list-tile-content>
          <v-list-tile-title v-text="item.title"></v-list-tile-title>
        </v-list-tile-content>
      </v-list-tile>
    </v-list>
    </v-navigation-drawer>
    <v-toolbar
      color="deep-purple lighten-1"
      clipped-left
      fixed
      app
      dark
    >
      <v-toolbar-side-icon @click="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
      <v-spacer />
    </v-toolbar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <v-footer
      fixed
      app
    >
      <span>&copy; MiEstilista 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      items: [
        {
          icon: 'assessment',
          title: 'Principal',
          to: '/'
        },
        // {
        //   icon: 'event_available',
        //   title: 'Citas',
        //   to: '/citas'
        // },
        {
          icon: 'settings',
          title: 'Configuraciones',
          to: '/config'
        }
      ],
      right: true,
      title: 'Mi Estilista (Admin)'
    }
  }
}
</script>
