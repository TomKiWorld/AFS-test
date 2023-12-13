<template>
  <div id="app">
    <div
      id="nav"
      class="nav center-text"
    >
      <router-link to="/">Home</router-link> |
      <router-link to="/transfers">Transfers</router-link>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--primary-color);
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: var(--primary-color);

    &.router-link-exact-active {
      color: var(--secondary-color);
    }
  }
}</style>
