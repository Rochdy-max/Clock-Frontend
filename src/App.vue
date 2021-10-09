<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
    <router-view/>
    <footer>
      <p>Clock Display with Vuejs</p>
    </footer>
  </div>
</template>

<style>
@font-face {
  font-family: 'Lato';
  src: url('../../../tmp/Lato-Regular.ttf')
}

#app {
  font-family: 'Lato', Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: rgb(0, 140, 255);
}
</style>
