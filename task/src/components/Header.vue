<template>
  <header class="header">
    <div class="container">
      <div class="header__inner">
        <img
          src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f4/Blued_Logo_appstore.svg/1200px-Blued_Logo_appstore.svg.png"
          alt="logo"
          class="header__logo"
        />
        <h1 class="header__title">Vue-tasks</h1>
        <label class="label">
          <div class="toggle">
            <input
              class="toggle-state"
              type="checkbox"
              name="check"
              v-model="dark"
              @change="$emit('changeTheme', dark)"
            />
            <div class="indicator"></div>
          </div>
        </label>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      dark: true,
    };
  },
  props: {
    msg: String,
  },
  methods: {
    changeTheme(e) {
      console.log(e);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.header {
  background-color: #1d73bc;
  color: white;
  &__inner {
    display: flex;
    justify-content: space-between;
    height: 60px;
    align-items: center;
    padding: 10px 0px;
  }
  &__logo {
    width: 50px;
    height: 50px;
  }
}

.label {
  display: inline-flex;
  align-items: center;
  cursor: pointer;
  color: #394a56;
}

.label-text {
  margin-left: 16px;
}

.toggle {
  isolation: isolate;
  position: relative;
  height: 30px;
  width: 60px;
  border-radius: 15px;
  overflow: hidden;
  background-color: gray;
}

.toggle-state {
  display: none;
}

.indicator {
  height: 100%;
  width: 200%;
  background: #ecf0f3;
  border-radius: 15px;
  transform: translate3d(-75%, 0, 0);
}

.toggle-state:checked ~ .indicator {
  transform: translate3d(25%, 0, 0);
}
.container {
  max-width: 1200px;
  margin: 0 auto;
}
</style>
