<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <a-button type="primary" @click="testClick()">Primary</a-button>

    <hr />
    <div v-for="(item, index) in menuData" :key="index">
      <div>
        <span>name={{ item }}</span>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "home",
  components: {
    HelloWorld
  },
  data() {
    const menuData = this.getMenuData(this.$router.options.routes);
    return {
      menuData
    };
  },
  methods: {
    testClick() {
      // this.$router.push({ path: "/dashboard/analysis" });
      // console.log(this.$router);
      // console.log(this.$route);
    },
    childClick() {
      console.log("yes");
    },
    getMenuData(routes) {
      const menuData = [];
      routes.forEach(item => {
        if (item.name && !item.hideInMenu) {
          const newItem = { ...item };
          delete newItem.children;
          if (item.children && !item.hideChildrenInMenu) {
            newItem.children = this.getMenuData(item.children);
          }
          menuData.push(newItem);
        } else if (
          !item.hideInMenu &&
          !item.hideChildrenInMenu &&
          item.children
        ) {
          menuData.push(...this.getMenuData(item.children));
        }
      });
      return menuData;
    }
  }
};
</script>
