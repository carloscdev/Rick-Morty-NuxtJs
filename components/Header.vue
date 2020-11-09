<template>
  <div class="header">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <nuxt-link class="listaMenu logo" to="/">Rick &amp; Morty</nuxt-link>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
        <b-navbar-nav>
          <nuxt-link class="listaMenu" to="/">Personajes</nuxt-link>
          <nuxt-link class="listaMenu" to="/" disabled>Episodios</nuxt-link>
        </b-navbar-nav>   

        </b-navbar-nav>
      </b-collapse>
  </b-navbar>
  </div>
</template>
<style>
  .listaMenu {
    color: white;
    margin-right: 3vw;
  }
  .logo{
    font-weight: 700;
    font-size: calc(1em + 5px);
  }
  .listaMenu:hover {
    color:azure;
  }
</style>