# vue-svg-icon

> 基于 svg-sprite 的 svg-icon 方案实践 Demo。在基于 vue-cli 生成的项目中，引入基于 svg-sprite 的 icon 方案。

**更新：该解决方案已经封装成npm包，可以通过npm install的方式直接安装使用。该demo仅作解决方案的示意。**

## npm安装地址

[svg-sprite-icon](https://www.npmjs.com/package/svg-sprite-icon)


## 在线预览

[预览，戳我](https://monster1935.github.io/vue-svg-icon/)


## 使用方式

一、安装 svg-sprite-loader, svgo, svgo-loader

```bash
npm install svg-sprite-loader --save-dev
npm install svgo svgo-loader --save-dev
```

二、配置webpack

假定icon路径存放在以 SvgIcon.vue 同级的文件夹 icons 中。

webpack.base.config.js

```javascript
// svgo 的精简规则配置文件
const svgoConfig = require('../config/svgo-config.json');

....
{
  test: /\.svg$/,
  include: [resolve('src/components/svg-icon/icons')],
  use: [
    {
      loader: 'svg-sprite-loader',
      options: {
        symbolId: 'icon-[name]'
      }
    },
    {
      loader: 'svgo-loader',
      options: svgoConfig,
    }
  ]
},
...
{
  test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
  loader: 'url-loader',
  exclude: [resolve('src/components/svg-icon/icons')], // 避免 url-loader 重复处理该文件夹下的内容
  options: {
    limit: 10000,
    name: utils.assetsPath('img/[name].[hash:7].[ext]')
  }
},
```

svgo-config.json

```json
{
  "plugins": [
    { "cleanupAttrs": true },
    { "cleanupEnableBackground": true },
    { "cleanupIDs": true },
    { "cleanupListOfValues": true },
    { "cleanupNumericValues": true },
    { "collapseGroups": true },
    { "convertColors": true },
    { "convertPathData": true },
    { "convertShapeToPath": true },
    { "convertStyleToAttrs": true },
    { "convertTransform": true },
    { "mergePaths": true },
    { "removeComments": true },
    { "removeDesc": true },
    { "removeDimensions": true },
    { "removeDoctype": true },
    { "removeEditorsNSData": true },
    { "removeEmptyAttrs": true },
    { "removeEmptyContainers": true },
    { "removeEmptyText": true },
    { "removeHiddenElems": true },
    { "removeMetadata": true },
    { "removeNonInheritableGroupAttrs": true },
    { "removeRasterImages": true },
    { "removeTitle": true },
    { "removeUnknownsAndDefaults": true },
    { "removeUselessDefs": true },
    { "removeUnusedNS": true },
    { "removeUselessStrokeAndFill": true },
    {
      "removeAttrs": { "attrs": "fill"}
    },
    { "removeXMLProcInst": true },
    { "removeStyleElement": true },
    { "removeUnknownsAndDefaults": true},
    { "sortAttrs": true }
  ]
}


```

三、SvgIcon.vue 的使用

SvgIcon.vue

```javascript
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
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>


```

业务组件中使用SvgIcon.vue

```javascript
import SvgIcon from '../components/svg-icon/index';

<div class="icon-loading">
  <svg-icon
    icon-class="loading"
    class-name="`svg-icon-loading`"
  >
  </svg-icon>
</div>

<style>
  div.icon-loading {
    color: #cdcdcd;
  }
</style>

```
