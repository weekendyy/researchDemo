<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.noselect {

-webkit-touch-callout: none; /* iOS Safari */

-webkit-user-select: none; /* Chrome/Safari/Opera */

-khtml-user-select: none; /* Konqueror */

-moz-user-select: none; /* Firefox */

-ms-user-select: none; /* Internet Explorer/Edge */

user-select: none; 

-webkit-tap-highlight-color:transparent;

}
</style>
