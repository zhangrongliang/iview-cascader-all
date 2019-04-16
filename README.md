# iview-cascader-all
从一组相关联的数据集合中进行选择，常用于省市区、公司级层、事务分类等。

> 拓展[iviewui](https://www.iviewui.com/)的[cascader](https://www.iviewui.com/components/cascader)组件,[参考](https://zhangrongliang.com/views/iviewui/iview-cascader-all.html)。

1. 增加多选
2. 增加全选

## 下载模块[iview-cascader-all](https://www.npmjs.com/package/iview-cascader-all)

```nodejs
yarn add iview-cascader-all || npm i iview-cascader-all
```

## API
| 属性         | 说明           | 类型     | 默认值 |
|------------ | -------------  | --------|-------|
| allToone    | 是否开启子级全选 | Boolean  | false  |
| oneToone    | 是否开启每级单选 | Boolean  | false  |
| data        | 可选项的数据源   | Array    |  []    |
| placeholder | 输入框占位符    | String    | 请选择  |


更多参考[Cascader](https://www.iviewui.com/components/cascader)

## 使用方法一：
一级多选，子集多选、全选。
属性：[:allToone ="true"]

```js
1.
import Vue from 'vue'
import iviewCascaderAll from 'iview-cascader-all'
Vue.component('iview-cascader-all', iviewCascaderAll)

2.
<template lang="html">
<div>
  <iview-cascader-all @on-change="changeText" :allToone="true" :data="data" placeholder="数据"></iview-cascader-all>
</div>
</template>

<script>
export default {
    data() {
        return {
            data: [],
            changeText: []
        }
    }
}
</script>

```  
#### 方法一配图
![iview.png](https://github.com/zhangrongliang/iview-cascader-all/blob/master/1.png?raw=true)

## 使用方法二：
每级单选，iview基础功能，待优化。可用
属性：[:oneToone ="true"]

```js
1.
import Vue from 'vue'
import iviewCascaderAll from 'iview-cascader-all'
Vue.component('iview-cascader-all', iviewCascaderAll)

2.
<template lang="html">
<div>
  <iview-cascader-all @on-change="changeText" :oneToone="true"  :data="data" placeholder="数据"></iview-cascader-all>
</div>
</template>

<script>
export default {
    data() {
        return {
            data: [],
            changeText: []
        }
    }
}
</script>

```  
#### 方法二配图
![iview.png](https://github.com/zhangrongliang/iview-cascader-all/blob/master/2.png?raw=true)

## 使用方法二：
默认，一级单选，子级多选。

```js
1.
import Vue from 'vue'
import iviewCascaderAll from 'iview-cascader-all'
Vue.component('iview-cascader-all', iviewCascaderAll)

2.
<template lang="html">
<div>
  <iview-cascader-all @on-change="changeText"  :data="data" placeholder="数据"></iview-cascader-all>
</div>
</template>

<script>
export default {
    data() {
        return {
            data: [],
            changeText: []
        }
    }
}
</script>

```  
#### 方法二配图
![iview.png](https://github.com/zhangrongliang/iview-cascader-all/blob/master/3.png?raw=true)


## 版本
### 1.0.3
- 修复全选N级选中切换BUG
- 修复清除功能BUG

### 1.0.2
- 修复父级切换，子级数据未保存的问题
- 修复父级数据切换，子级数据未排序问题
