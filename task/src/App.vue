<template>
  <div :class="'app ' + (dark ? 'dark' : '')">
    <Header @changeTheme="changeTheme($event)" />
    <Home />
    <Footer />
  </div>
</template>

<script>
import Home from "./views/Home";
import Header from "./components/Header";
import Footer from "./components/Footer";

export default {
  components: {
    Home,
    Header,
    Footer,
  },
  data() {
    return {
      dark: true,
    };
  },
  methods: {
    changeTheme(theme) {
      this.dark = theme;
      console.log(this.dark);
    },
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
}
body {
  min-width: 320px;
  font-family: sans-serif;
}
.app.dark {
  & .header {
    background-color: black !important;
  }
  & .home {
    background-color: #1a1a1a;
  }
}
</style>
