<template>
  <div id="app">
    <div id="带更改的robot">
      <ul>
        动画交互区
      </ul>
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

/********************************/

#带更改的robot {
  height: 200px;
  font-size:50px;
  background-color: #42b983;
}
</style>
