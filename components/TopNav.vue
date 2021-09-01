<template>
  <div>
    <b-navbar type="dark" variant="dark">
      <b-navbar-brand to="/">Test Manzana Verde</b-navbar-brand>
      <b-navbar-nav>
        <b-nav-item v-if="$auth.loggedIn" to="/">Inicio</b-nav-item>
        <b-nav-item v-if="!$auth.loggedIn" to="/registro">Registrarme</b-nav-item>
        <b-nav-item v-if="!$auth.loggedIn" to="/login">Iniciar Sesión</b-nav-item>
      </b-navbar-nav>
      <b-navbar-nav class="ml-auto">
        <b-button v-if="$auth.loggedIn" variant="danger" @click.prevent="logout">Cerrar Sesión</b-button>
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>
<script>
export default {
  methods: {
    async logout() {
      try {
        await this.$auth.logout('logout')
      } catch (e) {
        console.log(e);
      }
    },
  },
}
</script>
