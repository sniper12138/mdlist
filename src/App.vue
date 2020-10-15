<template>
  <div id="app">
    <router-view v-if="routerAlive" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      routerAlive: true,
    };
  },
  provide(){    //在父组件中创建属性
      return {
          routerRefresh: this.routerRefresh
      }
  },
  methods: {
    routerRefresh() {
      this.routerAlive = false;
      this.$nextTick(() => {
        this.routerAlive = true;
      });
    },
  },
};
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
