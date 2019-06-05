<template>
  <div style="width: 256px">
    <a-menu
      :defaultSelectedKeys="['/dashboard/analysis']"
      :defaultOpenKeys="['/dashboard']"
      mode="inline"
      :theme="theme"
      :inlineCollapsed="collapsed"
    >
      <template v-for="item in menuData">
        <a-menu-item
          v-if="!item.children"
          :key="item.path"
          @click="() => $router.push({ path: item.path, query: $route.query })"
        >
          <a-icon v-if="item.meta.icon" :type="item.meta.icon" />
          <span>{{ item.meta.title }}</span>
        </a-menu-item>
        <sub-menu v-else :menu-info="item" :key="item.path" />
      </template>
    </a-menu>
  </div>
</template>

<script>
/*
 * recommend SubMenu.vue https://github.com/vueComponent/ant-design-vue/blob/master/components/menu/demo/SubMenu.vue
 * SubMenu1.vue https://github.com/vueComponent/ant-design-vue/blob/master/components/menu/demo/SubMenu1.vue
 * */
import SubMenu from "./SubMenu";
import { check } from "../utils/auth";
export default {
  props: {
    theme: String
  },
  components: {
    "sub-menu": SubMenu
  },
  data() {
    const menuData = this.getMenuData(this.$router.options.routes);
    return {
      collapsed: false,
      menuData
    };
  },
  methods: {
    toggleCollapsed() {
      this.collapsed = !this.collapsed;
    },
    getMenuData(routes) {
      const menuData = [];
      for (let item of routes) {
        if (item.meta && item.meta.authority && !check(item.meta.authority)) {
          break;
        }
        if (item.name && !item.hideInMenu) {
          const newItem = { ...item };
          delete newItem.children;
          if (item.children && !item.hideChildrenInMenu) {
            newItem.children = this.getMenuData(item.children);
          }
          if (newItem.meta) menuData.push(newItem);
        } else if (
          !item.hideInMenu &&
          !item.hideChildrenInMenu &&
          item.children
        ) {
          menuData.push(...this.getMenuData(item.children));
        }
      }
      // routes.forEach(item => {
      //   if (item.name && !item.hideInMenu) {
      //     const newItem = { ...item };
      //     delete newItem.children;
      //     if (item.children && !item.hideChildrenInMenu) {
      //       newItem.children = this.getMenuData(item.children);
      //     }
      //     if (newItem.meta) menuData.push(newItem);
      //   } else if (
      //     !item.hideInMenu &&
      //     !item.hideChildrenInMenu &&
      //     item.children
      //   ) {
      //     menuData.push(...this.getMenuData(item.children));
      //   }
      // });
      return menuData;
    }
  }
};
</script>
