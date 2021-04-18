<template>
  <v-app>
    <v-app-bar app clipped-left fixed>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <router-link to="/" tag="h3" class="pointer ml-5"> Machination </router-link>
      <v-spacer />
      <span v-if="user" class="mr-5">{{ userDisplayName }}</span>
      <v-btn v-if="!user" outlined class="mr-5" @click="nav('login')"> Login </v-btn>
      <v-btn v-if="!user" outlined @click="nav('register')"> Register </v-btn>
      <v-btn v-if="user" @click="logout" outlined> Logout </v-btn>
    </v-app-bar>

    <v-content>
      <v-navigation-drawer v-model="drawer" app clipped disable-resize-watcher>
        <v-list nav>
          <v-list-item @click="nav('about')" style="justify-content: start">
            <v-icon class="mr-5"> mdi-information </v-icon>
            <span>About</span>
          </v-list-item>
          <v-list-item @click="nav('lights')">
            <v-icon class="mr-5"> mdi-gamepad-variant-outline </v-icon>
            <span>Lights Out</span>
          </v-list-item>
          <v-list-item>
            <v-icon class="mr-5" />
          </v-list-item>
          <v-list-item>
            <v-icon class="mr-5" />
          </v-list-item>
          <v-list-item>
            <v-icon class="mr-5" />
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
      <v-container fluid fill-height justify-center @click.stop="drawer = false">
        <router-view @click.stop="drawer = false" />
      </v-container>
    </v-content>

    <v-footer app>
      <span>&copy; 2020 Version 0.1.0 </span>
    </v-footer>
  </v-app>
</template>

<script>
import { mapState } from 'vuex';
export default {
  name: 'App',
  props: {},

  created() {
    this.$vuetify.theme.dark = true;
  },
  computed: {
    userDisplayName() {
      if (this.user) {
        return this.users.data.firstName + ' ' + this.users.data.lastName;
      }
      return '';
    },
    ...mapState(['user', 'users']),
  },
  data() {
    return {
      drawer: true,
    };
  },
  methods: {
    navigateHome() {
      this.$router.push({ name: 'Home' });
    },

    logout() {
      this.$store.commit('setUser', null);
      this.$store.dispatch('users/closeDBChannel', { clearModule: true });
      this.$firebase.auth().signOut();

      console.log(this.$firebase);

      // this.$firebase.auth().signOut().then(resp => { console.log('Logout Successful', resp) }).catch(err => { console.log(err) })
    },
  },
};
</script>
<style scoped>
.pointer {
  cursor: pointer;
}
</style>
