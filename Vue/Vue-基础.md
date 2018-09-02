# Vue 基础

### Vue实例

- **创建一个Vue实例**

  - 每个Vue应用都是通过Vue函数创建一个Vue实例开始；当创建一个Vue实例时，可以传入一个选项对象，如下代码

    ```javascript
    var vm = new Vue({
        // 选项
    })
    ```

- **数据与方法**

  - 当Vue实例被创建时，其添加的`data`对象中的属性是**响应式**的，如下代码

    ```javascript
    // 数据对象
    var data = {
        name: 'xxx'
    }
    
    // 将数据对象加入到Vue实例
    var vm = new Vue({
        data: data
    })
    
    // Vue实例会得到这个数据对象的属性
    vm.name === data.name // true
    
    // 设置Vue实例属性会影响到原始数据对象
    vm.name = 'yyy'
    console.log(data.name) // yyy
    
    // 设置原始数据对象属性也会影响到Vue实例属性
    data.name = 'zzz'
    vm.name // zzz
    ```

  - 除了数据属性，Vue实例还暴露一些有用的实例属性和方法，它们带有前缀`$`，如下代码

    ```javascript
    var data = {
        name: 'xxx'
    }
    var vm = new Vue({
        el: '#app',
        data: function() {
            return data
        }
    })
    
    vm.$data === data // true
    vm.$el === document.querySelector('#app') // true
    // $watch 是一个实例方法
    vm.$watch('name',function(newVal,oldVal) {
        // 这个回调将在`vm.name`改变后调用
    })
    ```

- **实例生命周期钩子**

  - 每个Vue实例在被创建时都要经过一系列的初始化过程；比如，需要设置数据监听、编译模板、将实例挂载到DOM并在数据变化时更新DOM。**在这些过程中会运行一些叫做生命周期钩子的函数，可以让用户在不同的阶段添加自己的代码**

    ```javascript
    // 比如 created 钩子可以用来在一个实例被创建后执行代码
    new Vue({
        el: '#app',
        data: function() {
            return {
                name: 'xxx'
            }
        },
        created: function() {
            // this 指向vm实例
            console.log('name is ' + this.name)
        }
    })
    
    // 还有其它的钩子在实例生命周期的不同阶段被调用，
    // 如 mounted、updated、destroyed
    ```

  - **生命周期钩子的`this`上下文指向调用它的Vue实例**。注意：不要在 选项属性或者回调上使用箭头函数，比如`created: () => console.log(this.name)`，这会导致`this`不是预期的Vue实例

### 模板语法

- **插值**

  - 文本插值—>使用双大括号来绑定数据，如下代码

    ```HTML
    <h1>
        {{ title }}
    </h1>
    <!-- 双大括号会被替换成对应数据对象上title属性的值 -->
    <!-- 无论何时，绑定的数据对象上title属性发生了改变，插值处的内容都会更新 -->
    ```

  - 原始 HTML--->为了输出HTML，需要使用`v-html`指令，如下代码

    ```html
    <div>
        <span v-html="rawHtml"></span>
    </div>
    <!-- 这个span会被替换成属性值rawHtml所包含的html内容 -->
    ```

  - **特性—>双大括号不能用在HTML 特性上，这个时候使用`v-bind`指令**，如下代码

    ```html
    <a v-bind:href="url"></a>
    ```

  - 使用 JavaScript 表达式--->对于所有的数据绑定，都可以使用 JavaScript 表达式，但是每个绑定只能包含**单个表达式**，如下代码

    ```javascript
    // 合格的单个表达式
    {{ number + 1 }}
    {{ ok ? 'yes' : 'no' }}
    // <div v-bind:id="'list-' + id"></div>
    
    // 这是语句，不是表达式
    {{ var a = 1 }}
    
    // 流控制也不会生效，比如if语句，请使用三元表达式
    ```

- **指令**

  - **指令是指带有`v-`前缀的特殊特性；指令特性的值预期是个单个JavaScript表达式；指令的作用是：当表达式的值改变时，将其产生的连带影响，响应式地作用于DOM**

    ```html
    <!-- v-if 指令将根据表达式 seen 的值的真假来插入或者移除 div 元素 -->
    <div v-if="seen">xxx</div>
    ```

  - **参数--->一些指令能够接收一个“参数”，在指令后面以冒号表示**，如下代码

    ```html
    <div class='btn' v-on:click="doSomething">btn</div>
    ```

  - **修饰符—>以`.`跟在参数后面的特殊后缀，用于指出一个指令应该以特殊方式绑定**，如下代码

    ```html
    <!-- 
    .prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault() 
    -->
    <form v-on:submit.prevent="onSubmit">...</form>
    ```

  - 缩写

    ```html
    <!-- v-bind -->
    <!-- 完整语法 -->
    <a v-bind:href="url">...</a>
    
    <!-- 缩写 -->
    <a :href="url">...</a>
    
    
    <!-- v-on -->
    <!-- 完整语法 -->
    <a v-on:click="doSomething">...</a>
    
    <!-- 缩写 -->
    <a @click="doSomething">...</a
    ```

### 计算属性和侦听器

- **计算属性**

  - 当在模板中放入太多的逻辑会让模板过重且难以维护，对于任何复杂逻辑，应使用计算属性

  - 基础例子

    - html代码如下

      ```html
      <div id="app">
          <p>original msg: {{ msg }}</p>
          <p>computed msg: {{ reversedMsg }}</p>
      </div>
      ```

    - js代码如下

      ```javascript
      var app = new Vue({
          el: '#app',
          data: function() {
              return {
                  msg: 'hello world'
              }
          },
          computed: {
              // 声明一个计算属性 reverseMsg
              reversedMsg: function() {
                  // this 指向 app实例
                  return this.msg.split('').reverse().join('')
              }
          }
      })
      
      // Vue知道 app.reversedMsg 依赖于 app.msg
      // 当 app.msg 发生改变时，所有依赖 app.reversedMsg 的绑定也会更新
      ```

  - 计算属性 vs 方法

    - 要实现上面效果也可以通过表达式调用方法，如下代码

      ```js
      // html
      // <p>methods msg: {{ reveredMsg() }}
      
      // 在组件中
      methods: {
          reveredMsg: function() {
              return this.msg.split('').reverse().join('')
          }
      }
      ```

    - 那么计算属性和方法的不同是：

      - **计算属性是基于它们的依赖进行缓存的，计算属性只有在它的相关依赖发生改变时才会重新求值。这表示只要`msg`还没有发生改变，多次访问`reveredMsg`计算属性会立即返回之前的计算结果，而不必再次执行函数**
      - **而调用方法则总会再次执行函数**

  - 计算属性 vs 侦听属性

    - 侦听属性--->用于观察和响应Vue实例上的数据变动
    - 当有一些数据需要随着其它数据变动而变动时，可以使用计算属性而不要滥用`watch`回调

- **侦听器—>当需要在数据变化时执行异步或开销较大的操作时，应使用`watch`选项**

### Class与Style绑定

- **绑定 HTML Class--->`v-bind:class`**

  - 对象语法—>可以把一个对象传给`v-bind:class`，以动态地切换class。如下代码

    ```html
    <!--下面这句表示 active 这个 class 是否存在取决于数据对象上的 isActive 属性是否为真 -->
    <div v-bind:class="{active: isActive}"></div>
    
    <!-- 可以在对象中传入多个属性来动态切换多个 class -->
    <div v-bind:class="{active: isActive, 'text-danger': hasError}"></div>
    <!-- 当isActive、hasError都为 true 时，渲染结果如下 -->
    <div class="active text-danger"></div>
    
    <!-- 绑定的数据对象不必内联定义在模板里，可以定义在数据对象里，如下 -->
    <div v-bind:class="classObj"></div>
    <!--
    data: function() {
        return {
            classObj: {
    			active: true,
    			'text-danger': false
            }
        }
    }
    -->
    
    <!-- 也可以将 classObj 绑定到一个返回对象的计算属性里 
    data: {
    	isActive: true,
    	error: null
    },
    computed: {
        classObj: function() {
            return {
                active: this.isActive && !this.error,
                'text-danger': this.error && this.error.type === 'fatal'
            }
        }
    }
    -->
    ```

  - 数组语法—>可以把一个数组传给`v-bind:class`，以应用一个class列表。如下代码

    ```HTML
    <div v-bind:class="[activeClass,errorClass]"></div>
    <!-- 对应的js代码 
    data: function() {
        return {
    		activeClass: 'active',
    		errorClass: 'text-danger'
        }
    }
    -->
    <!-- 渲染结果如下 -->
    <div class="active text-danger"></div>
    
    <!-- 根据条件切换列表中的class，可以使用三元表达式 -->
    <div v-bind:class="[isActive ? activeClass : errorClass]"></div>
    
    <!-- 数组语法中也可以使用对象语法 -->
    <div v-bind:class="[{active: isActive},errorClass]"></div>
    ```

  - 用在组件上—>当在一个自定义的组件上使用`class`属性时，这些类将被添加到该组件的根元素上；这个元素已经存在的类不会被覆盖

    ```javascript
    // 声明一个组件
    Vue.component('app',{
        template: '<div class="xxx sss">vue</div>'
    })
    // 使用组件时添加一些class
    // <app class="jjj kkk"></app>
    //html 渲染如下
    // <div class="xxx sss jjj kkk">vue</div>
    ```

- **绑定内联样式--->`v-bind:style`**

  - 对象语法--->CSS属性名可以使用驼峰式或短横线分割（使用单引号括起来）

    ```html
    <!-- 直接使用对象 -->
    <div v-bind:style="{color: xxx, 'background-color': yyy}"></div>
    
    <!-- 绑定一个样式对象 -->
    <div v-bind:style="styleObj"></div>
    <!-- 
    data: {
        styleObj: {
            color: '#fff',
            'background-color': 'pink'
        }
    }
    -->
    ```

  - 数组语法—>可以将多个样式对象应用到同一个元素上

    ```html
    <div v-bind:style="[commonStyle, specialStyle]"></div>
    ```

### 条件渲染

- **`v-if`指令--->根据表达式的值是true或者false来生成或者移除一个元素，操作的是DOM**

  ```html
  <!-- 渲染一个元素 -->
  <h1 v-if="seen">can you see here</h1>
  
  <!-- 渲染多个元素使用 template 元素 -->
  <template v-if="isActive">
  	<h1>hello world</h1>
      <div>xxx</div>
  </template>
  ```

- `v-else`指令表示`v-if`的else块，`v-else`必须紧跟在带`v-if`或者`v-else-if`的元素后面

  ```html
  <div v-if="Math.random() > 0.5">
      now you can see me
  </div>
  <div v-else>
      now you do not
  </div>
  ```

- `v-else-if`指令表示else-if块，`v-else-if`必须紧跟在带`v-if`或者`v-else-if`的元素后面

  ```html
  <div v-if="num === 1">
    1
  </div>
  <div v-else-if="num === 2">
    2
  </div>
  <div v-else-if="num === '3">
    3
  </div>
  <div v-else>
    not 1/2/3
  </div>
  ```

- **用`key`管理可复用的元素**

  - Vue会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。比如，可以允许用户在不同的登录方式之间切换，如下代码

    ```html
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address">
    </template>
    
    <!--
    在上面的代码中切换loginType不会清除用户已经输入的内容
    因为两个模板使用了相同的input，input不会被替换掉——仅仅是替换了它的placeholder
    -->
    ```

  - **可以添加`key`属性（具有唯一值）来表示两个相同的元素是完全独立的**

    ```html
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Enter your username" key="username-input">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Enter your email address" key="email-input">
    </template>
    
    <!--
    此时，两个input是完全独立的
    每次切换时，输入框的内容都会被重新渲染
    而label元素仍会被高效地复用，因为没有添加 key 属性
    -->
    ```

- **v-show**

  - 带有`v-show`的元素始终会被渲染并保留在DOM中；**`v-show`指令切换的是元素的CSS属性display**

    - 当`v-show`传入的值是true时，对应的DOM元素的display属性的值为block
    - 当`v-show`传入的值是false时，对应的DOM元素的display属性的值为none

  - `v-show`不支持`<template>`元素，也不支持`v-else`

  - `v-if`和`v-show`的不同

    >`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建
    >
    >`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换

  - `v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好

- **v-if和v-for一起使用**

  - 当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级

### 列表渲染

- **`v-for`指令根据一组数组渲染成一个列表**

  - html代码如下

    ```html
    <ul id="app">
        <!-- items 是源数据数组，item 是数组元素迭代的别名 -->
        <li v-for="item in items">
        	{{ item.msg }}
        </li>
    </ul>
    ```

  - js代码如下

    ```javascript
    var app = new Vue({
        el: '#app',
        data: function(){
            return {
                items: [
                    {msg: "111"},
                    {msg: "222"},
                    {msg: "333"}
                ]
            }
        }
    })
    ```

  - 渲染结果如下图

    ![](http://p7e35vgip.bkt.clouddn.com/v-for.png)

  - **在`v-for`块中，拥有对父作用域属性的完全访问权限。`v-for`还支持一个表示当前项索引的参数**

    - html代码如下

      ```html
      <ul id="app">
          <li v-for="(item,index) in items">
          	{{ parentMsg }} - {{ index }} - {{ item.msg }}
          </li>
      </ul>
      ```

    - js代码如下

      ```javascript
      var app = new Vue({
          el: '#app',
          data: function() {
              return {
                  parentMsg: "parent",
                  items: [
                      {msg: "111"},
                      {msg: "222"},
                      {msg: "333"}
                  ]
              }
          }
      })
      ```

    - 渲染结果如下

      ![](http://p7e35vgip.bkt.clouddn.com/index.png)

  - **可以使用`of`代替`in`作为分隔符，因为它是最接近 JavaScript 迭代器的语法**

    ```html
    <li v-for="item of items"></li>
    ```

- **一个对象的`v-for`，即用`v-for`通过一个对象的属性来迭代。参数有：值-value、键名-key、索引-index**

  - html代码如下

    ```html
    <ul id="app">
        <li v-for="(value,key,index) in object">
        	{{ index }} - {{ key }} : {{ value }}
        </li>
    </ul>
    ```

  - js代码如下

    ```javascript
    var app = new Vue({
        el: '#app',
        data: function() {
            return {
                object: {
                    firstName: 'John',
                    lastName: 'Doe',
                    age: 30
                }
            }
        }
    })
    ```

  - 渲染结果如下

    ![](http://p7e35vgip.bkt.clouddn.com/vfor-obj.png)

- **`key`**

  - 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，需要为每项提供一个唯一 `key` 属性；理想的 `key` 值是每项都有的且唯一的 id

  - **尽可能在使用`v-for`的时候提供`key`

    ```html
    <div v-for="item in items" v-bind:key="item.id"></div>
    ```

- **数组更新检测**

  - Vue 包含一组观察数组的变异方法，它们会改变原数组，所以它们也将会触发视图更新

    - `push()`
    - `pop()`
    - `shift()`
    - `unshift()`
    - `splice()`
    - `sort()`
    - `reverse()`

  - 替换数组

    - `filter()`, `concat()` 和 `slice()`不会改变原始数组，但总是返回一个新数组
    - Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的、启发式的方法，所以**用一个含有相同元素的数组去替换原来的数组是非常高效的操作**

  - 注意事项，由于 JavaScript 的限制，Vue 不能检测以下变动的数组

    - 当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`

    - 当你修改数组的长度时，例如：`vm.items.length = newLength`

      ```javascript
      var vm = new Vue({
          el: '#app',
          data: {
              items: ['a','b','c']
          }
      })
      // 第一类
      vm.items[1] = 'd' // 不是响应性的
      // 第二类
      vm.items.length = 2 // 不是响应性的
      
      
      // 解决第一类问题
      // Vue.set
      Vue.set(vm.items,index,newValue)
      
      // Array.prototype.splice
      vm.items.splice(index, 1, newValue)
      
      // 解决第二类问题
      vm.items.splice(newLength)
      ```

- **对象更改注意检测事项**

  - 由于 JavaScript 的限制，**Vue 不能检测对象属性的添加或删除**

    ```javascript
    var vm = new Vue({
        el: '#app',
        data: {
            a: 1
        }
    })
    // vm.a是响应性的
    
    vm.b = 2 // vm.b不是响应性的
    ```

  - 对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但可以使用 `Vue.set(object, key, value)` 方法向嵌套对象添加响应式属性

    ```javascript
    Vue.set(vm.object, 'age', 27)
    ```

  - 为已有对象赋予多个新属性，比如使用 `Object.assign()`

    ```javascript
    vm.userProfile = Object.assign({}, vm.userProfile, {
      age: 27,
      favoriteColor: 'Vue Green'
    })
    ```

- **显示过滤/排序结果**

  - 

