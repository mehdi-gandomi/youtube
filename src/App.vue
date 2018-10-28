<template>
  <div id="app">
    <main-header/>
    <router-view/>
    <main-footer/>
  </div>
</template>

<script>
import MainFooter from "@/components/partials/footer";
import MainHeader from "@/components/partials/header";
export default {
  name: 'App',
  components:{
        MainHeader,MainFooter
  },
}
</script>

<style lang="scss">
  
  @import "@/assets/scss/main.scss";
</style>
