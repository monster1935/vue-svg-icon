<!-- svg-sprite组件，业务组件中直接使用该组件展示icon -->
<template>
  <svg :class="svgClass" aria-hidden="true">
    <use :xlink:href="iconName"></use>
  </svg>
</template>

<script>
// 引入所有的svg的文件
const requireAll = requireContext => requireContext.keys().map(requireContext);
const req = require.context('./icons', false, /\.svg$/);
requireAll(req);

export default {
  name: 'svg-icon',
  props: {
    iconClass: {
      type: String,
      required: true,
    },
    className: {
      type: String,
    },
  },
  computed: {
    iconName() {
      return `#icon-${this.iconClass}`;
    },
    svgClass() {
      if (this.className) {
        return `svg-icon ${this.className}`;
      }
      return 'svg-icon';
    },
  },
};
</script>

<style scoped>
.svg-icon {
  width: 40px;
  height: 40px;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>
