<template>
  <NavbarVue :logo="logo_src" :alt="app_name"/>
  <router-view/>
  <FooterVue/>
</template>
<script setup>
import NavbarVue from './components/Navbar.vue';
import FooterVue from './components/Footer.vue';

const logo_src = "/img/logo.png"
const app_name = "Make your Burger"

</script>
<style>
*{
  font-family: Helvetica ;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.main-container{
  min-height: 250px;
}

h1{
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
