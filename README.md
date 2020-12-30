# LearnVuejsPart02
学习组件化开发

### 一.资料整理来源  
coderwhy老师  B站账号：ilovecoding  
bilibili URL：https://space.bilibili.com/36139192  
视频(52-75p) URL：https://www.bilibili.com/video/BV15741177Eh?p=1
  
# 二、本部分知识大纲
#  
(数字表示视频URL分p)
### 一、组件化开发原理和基本使用 (52-54)
#### 1.1 创建组件构造器对象  
* component组件，简写cpn
* 组件构造器, cpncon=Vue.extend({})  
* 模板属性，template:``，ES6语法，可以换行定义字符串等操作
#### 1.2 注册组件  
* 全局组件，可以在多个Vue对象里使用, Vue.component('hicpn',cpncon)
* 局部组件，在Vue实例中的属性components:{组件标签Tag名:构造器名}  
#### 1.3 使用组件
* <hicpn></hicpn>或<hicpn\>
* 注意全局和局部组件的作用域

### 二、父子组件、语法糖和模板分离 (55-58)
#### 2.1 父组件和子组件
* 先构造子组件cpnc2,再父组件cpnc1
* cpnc1组件构造器的components里注册组件cpnc2，template里使用
* Vue实例也是一个组件

#### 2.2 组件的语法糖注册
* 全局组件的语法糖 构造并注册组件
```javascript
Vue.component('cpn1', {
    template: `...`
    })
```
* 局部组件的语法糖，Vue实例或父组件的属性里
```javascript
components: {
      'cpn2': {
        template: `...`
      }
    }
```

#### 2.3 组件模板的分离
* 方法一，script的类型text/x-template
```html
<script type="text/x-template" id="cpn1">
  模板代码
</script>
```
* 方法二，template标签，两种方法都要加id进行调用
```html
<template id="cpn2">
  模板代码
</template>
```

