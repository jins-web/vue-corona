<template>
  <h2 class="content-title">{{ content.label }}</h2>
  <component :is="targetComponent"></component>
</template>

<script>
import { defineAsyncComponent } from "vue";
export default {
  name: "mainContent",
  props: {
    content: {
      type: Object,
      default() {
        return {
          label: "test menu",
          path: "test",
          index: "testindex",
        };
      },
    },
  },
  data() {
    return {
      targetComponent: null,
    };
  },
  watch: {
    content: {
      deep: true,
      handler: function (newValue) {
        this.targetComponent = defineAsyncComponent(() =>
          import(`@/components/${newValue.path}/${newValue.index}`)
        );
      },
    },
  },
};
</script>

<style></style>
