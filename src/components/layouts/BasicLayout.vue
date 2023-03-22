<template>
  <div class="common-layout">
    <el-container class="main">
      <el-header><BasicHeader /></el-header>
      <el-container>
        <el-aside width="menuWidth"
          ><SideMenu :isCollapse="isCollapse"
        /></el-aside>
        <el-container>
          <el-main>Main</el-main>
          <el-footer>Footer</el-footer>
        </el-container>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import { computed } from "vue";
import BasicHeader from "./BasicHeader";
import SideMenu from "./SideMenu";
import useBreakpoint from "@/composables/useBreakpoint";
export default {
  name: "basic-layout",
  components: {
    BasicHeader,
    SideMenu,
  },
  setup() {
    const { type } = useBreakpoint();
    const isCollapse = computed(() => type.value === "sm");
    const menuWidth = computed(() => (isCollapse.value ? "65px" : "200px"));
    return {
      isCollapse,
      menuWidth,
    };
  },
};
</script>

<style>
.main,
.common-layout {
  height: 100%;
}
</style>
