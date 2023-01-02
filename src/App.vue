<template>
    <NavBar :alt="nome_app" :logo="logo_src"/>
    <router-view/>
    <Footer />
</template>

<script>
import NavBar from './components/NavBar.vue';
import Footer from './components/Footer.vue';
export default {
  name: 'App',
  components: {
    NavBar,
    Footer
  },
  data (){
    return{
      nome_app: "Ambburgueria",
      logo_src: "/img/logo.png"
    }
  }
}
</script>

<style>
  *{
    font-family: helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  .main-container{
    margin: 50px;
    min-height: 250px;
  }

  h1{
    font-size: 42px;
    color: #222;
    text-align: center;
    margin-bottom: 32px;
  }
</style>