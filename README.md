# LearnVuejsPart02
学习组件化开发，使用代码注意js的路径！

### 一.资料整理来源  
coderwhy老师  B站账号：ilovecoding  
bilibili URL：https://space.bilibili.com/36139192  
视频(52-75p) URL：https://www.bilibili.com/video/BV15741177Eh?p=52
  
# 二、本部分知识大纲
(数字表示视频URL分p)
### 一、组件化开发原理和基本使用 (52-54)
-----------文件夹 01-组件化开发 知识-----------  
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
* 方法二(常用)，template标签，两种方法都要加id进行调用，还要加一个根标签
```html
<template id="cpn2">
  模板代码
</template>
```

#### 2.4 组件的数据存放
* 组件的data属性必须是函数，返回一个新的对象
```javascript
data(){
  //组件不是共享的一个对象，要返回一个新的对象
  return{
    cnt:0
  }
}
```

### 三、父子组件的通信 (59-63)
#### 3.1 父传子 props，3种定义方法
* 1.数组传递。不会检测数据类型，都用''括起来
```javascript
  props: ['getmsg', 'getcolors']
```
* 2.类型限制。可以是默认类型，也可是自定义类
```javascript
//默认类型有：String、Number、Boolean、Array、Object、Date、Function、Symbol等
  getmsg:String,
  getcolors:Array
```
* 3.类型限制和默认值。传递数组时，将default作为函数，返回一个数组
```javascript
getmsg:{
  type:String,
  default:'hello 哇',
  //required必要条件属性，bool值，缺少就报错
  //required:true
},
getcolors:{
  type: Array,
  default(){
    return []
  },
}
```
#### 3.2 父传子props处理驼峰标识
* 定义传递的数据名为驼峰标识时，如
    * cInfo
    * myChildNumber
* 在使用父组件监听属性时，对应用：
    * <cpn :c-info="info" :my-child-number="num"></cpn>

#### 3.3 子传父 $emit()自定义事件方法
* 1.定义方法
```javascript
//子组件的按钮点击方法
btnClick(cate){
  //$emit(自定义事件名,点击的元素)
  this.$emit('childclick',cate)
}
//父组件的监听方法
cpnClick(item){
  //打印元素名
  console.log('cpn is click',item.name)
}
```
* 2.使用方法
```html
<!-- 子组件的模板先通过categories数组生成按钮，每个都调用点击方法 -->
<button v-for="cate in categories" @click="btnClick(cate)">
  {{cate.name}}
</button>
<!-- 父组件监听，并使用父的自定义事件方法childclick,没有使用参数自动传监听的素对象--> -->
<cpn @childclick="cpnClick"></cpn>
```

### 四、案例:父子组件的双向绑定 (64-66)
#### 4.1 需求：(本案例模拟了一个汇率的功能)
知识点：  
* 1.子组件先接收父组件数据，再通过输入框双向绑定子组件数据。
* 2.将输入框的值再绑定父组件数据。
* 3.将数据1、数据2始终同步成100倍关系

#### 4.2 案例的详细图解
视频 URL：https://www.bilibili.com/video/BV15741177Eh?p=65

#### 4.3 (了解)组件的watch属性，监听数据的改变

### 五、父子组件的访问方式 (67-68)
#### 5.1 父访问子方式：$children、$refs
在父组件的按钮methods的里：  
```javascript
btnClick(){
  //1. $children，所有子组件，数组,一般不用下标访问，避免添加组件时引起的错位
  console.log(this.$children[0].name)
  //2. $refs,添加ref属性，在多个相同子组件时，（$refs.属性值）访问特定子组件
  console.log(this.$refs.aaa)
}
```
#### 5.2 子访问父方式：$parent、$root
在子组件的按钮methods的里：  
```javascript
cbtnCk(){
  //1. $parent,访问父组件。缺点：耦合度太高，复用性不足
  console.log(this.$parent)
  console.log(this.$parent.name)
  //2. $root，访问根节点Vue。Vue实例的数据很少
  console.log(this.$root.name)
}
```
  
-----------文件夹 02-组件化高级 知识-----------  
### 一、slot插槽的使用 (69-72)
#### 1.1 什么是组件的slot插槽
插槽的详解博客 URL：https://blog.csdn.net/hui_style/article/details/103484378  
  
#### 1.2 单个插槽
* 模板的slot里可以设置默认的标签元素
* 使用时，slot会被替换成cpn标签内的所有元素
```html
<!-- 使用组件时 -->
<cpn></cpn>
<cpn>
  <p>插槽p标签</p>
</cpn>
<!-- 有1个slot的模板 -->
<template id="cpn">
  <div>
    <h2>我是组件</h2>
    <p>我是组件，哈哈哈</p>
    <!-- 默认标签<h2> -->
    <slot><h2>插槽默认标签元素</h2></slot>
  </div>
</template>
```
  
#### 1.3 具名插槽
* 模板里使用多个插槽时，为slot添加name属性
* 使用时指定元素的slot属性的name值，不指定就替换全部slot
```html
<!-- 使用组件时 -->
<cpn>
  <button slot="left">返回键</button>
  <span slot="center">标题</span>
  <button slot="right">菜单键</button>
</cpn>
<!-- 多个slot的模板 -->
<template id="cpn">
  <div>
    <slot name="left"><span>左边</span></slot>
    <slot name="center"><span>中间</span></slot>
    <slot name="right"><span>右边</span></slot>
  </div>
</template>
```
  
#### 1.4 编译作用域
* 父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译  
  
#### 1.5 作用域插槽
* 1.父组件里不能直接使用子组件数据
* 2.子组件的作用域插槽通过自定义属性v-bind绑定子组件的数据
```html
  <!-- 绑定自定义属性langs -->
  <slot :langs="pLangs">
  <!-- 子组件的data()的数据 -->
  pLangs:['C/C++','C#','Python','Java','PHP','JavaScript']
```
* 3.父组件使用template的slot-scope属性(自定义值)接收子组件的数据，使用（插槽变量.子模板的数据名）接收
```html
<cpn>
  <template slot-scope="slot">
    <span v-for="item in slot.langs">{{item}} - </span>
  </template>
</cpn>
```
  
-----------文件夹 03-前端模块化 知识-----------  
  