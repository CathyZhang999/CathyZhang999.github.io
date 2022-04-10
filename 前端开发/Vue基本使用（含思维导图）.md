- [1、动态属性等](#1动态属性等)
    - [文本插值](#文本插值)
    - [JS表达式](#js表达式)
    - [动态属性](#动态属性)
    - [v-html](#v-html)
- [2、 computed 计算属性](#2-computed-计算属性)
- [3、watch监听](#3watch监听)
- [4、class和style](#4class和style)
- [5、条件渲染](#5条件渲染)
- [6、循环列表渲染](#6循环列表渲染)
- [7、事件](#7事件)
    - [7.1 event参数](#71-event参数)
    - [7.2  一个事件调用多个函数](#72--一个事件调用多个函数)
    - [7.3  修饰符](#73--修饰符)
- [8、表单 v-model双向数据绑定](#8表单-v-model双向数据绑定)
    - [8.1 输入框](#81-输入框)
    - [8.2 多行文本](#82-多行文本)
    - [8.3 复选框](#83-复选框)
    - [7.4 多个复选框](#74-多个复选框)
    - [8.5  radio单选](#85--radio单选)
    - [8.6 下拉列表单选](#86-下拉列表单选)
    - [8.7 下拉列表多选](#87-下拉列表多选)
    - [8.8 表单修饰符](#88-表单修饰符)
- [9、思维导图](#9思维导图)
## 1、动态属性等
#### 文本插值

```javascript
<p>文本插值 {{message}}</p>
```

####  JS表达式

```javascript
<p>JS 表达式 {{ flag ? 'yes' : 'no' }} </p>
```
#### 动态属性
```javascript
<p :id="dynamicId">动态属性 id</p>
```
#### v-html
```javascript
 <p v-html="rawHtml">有 xss 风险</p>
```
<font size=3>&emsp;&emsp;【注意】使用 v-html 之后，将会覆盖子元素；同时有XSS风险
```javascript
<template>
    <div>
        <p>文本插值 {{message}}</p>
        <p>JS 表达式 {{ flag ? 'yes' : 'no' }} （只能是表达式，不能是 js 语句）</p>
        <p :id="dynamicId">动态属性 id</p>
        <hr/>
        <p v-html="rawHtml">
            <span>有 xss 风险</span>
            <span>【注意】使用 v-html 之后，将会覆盖子元素</span>
        </p>       
    </div>
</template>

<script>
export default {
    data() {
        return {
            message: 'hello vue',
            flag: true,
            rawHtml: '指令 - 原始 html <b>加粗</b> <i>斜体</i>',
            dynamicId: `id-${Date.now()}`
        }
    }
}
</script>
```
## 2、 computed 计算属性
<font size=3>&emsp;&emsp;computed有缓存，data不变则不会重新计算
```javascript
<template>
    <div>
        <p>num {{num}}</p>
        <p>double1 {{double1}}</p>
        <input v-model="double2"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            num: 20
        }
    },
    computed: {
        double1() {
            return this.num * 2
        },
        double2: {
            get() {
                return this.num * 2
            },
            set(val) {
                this.num = val/2
            }
        }
    }
}
</script>
```


## 3、watch监听

 - 值类型，可正常拿到 oldVal 和 val
 - 引用类型，拿不到 oldVal 。因为指针相同。
 - 引用类型深度监听: deep: true

```javascript
<template>
    <div>
        <input v-model="name"/>
        <input v-model="info.city"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: 'TOM',
            info: {
                city: '北京'
            }
        }
    },
    watch: {
        name(oldVal, val) {
            // eslint-disable-next-line
            console.log('watch name', oldVal, val) // 值类型，可正常拿到 oldVal 和 val
        },
        info: {
            handler(oldVal, val) {
                // eslint-disable-next-line
                console.log('watch info', oldVal, val) // 引用类型，拿不到 oldVal 。因为指针相同，此时已经指向了新的 val
            },
            deep: true // 深度监听
        }
    }
}
</script>
```

## 4、class和style
 - class可以使用动态属性
 - style需用驼峰写法

```javascript
<template>
    <div>
        <p :class="{ black: isBlack, yellow: isYellow }">使用 class</p>
        <p :class="[black, yellow]">使用 class （数组）</p>
        <p :style="styleData">使用 style</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            isBlack: true,
            isYellow: true,

            black: 'black',
            yellow: 'yellow',

            styleData: {
                fontSize: '40px', // 转换为驼峰式
                color: 'red',
                backgroundColor: '#ccc' // 转换为驼峰式
            }
        }
    }
}
</script>

<style scoped>
    .black {
        background-color: #999;
    }
    .yellow {
        color: yellow;
    }
</style>
```

## 5、条件渲染

 1. 可用变量或表达式
 2. v-if与v-show的区别： 
<font size=3>**v-if：为否时，则渲染无此DOM节点**
<font size=3>**v-show：为否时，则其display属性为none**

 3. 使用场景
<font size=3>v-if：只是一次性选择或DOM节点切换不频繁时
<font size=3>v-show：多个DOM节点频繁切换显示时

## 6、循环列表渲染

 - 用v-for遍历数组或对象 
 - v-for与v-if不用同时用在同一元素上，v-for的优先级高
 -    **key：为每个节点设定的唯一标识，作用是可高效的更新虚拟DOM**

```javascript
<template>
    <div>
        <p>遍历数组</p>
        <ul>
            <li v-for="(item, index) in listArr" :key="item.id">
                {{index}} - {{item.id}} - {{item.title}}
            </li>
        </ul>
        <p>遍历对象</p>
        <ul >
            <li v-for="(val, key, index) in listObj" :key="key">
                {{index}} - {{key}} -  {{val.title}}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            flag: false,
            listArr: [
                { id: 'a', title: '标题1' }, // 数据结构中，最好有 id ，方便使用 key
                { id: 'b', title: '标题2' },
                { id: 'c', title: '标题3' }
            ],
            listObj: {
                a: { title: '标题1' },
                b: { title: '标题2' },
                c: { title: '标题3' },
            }
        }
    }
}
</script>
```

## 7、事件

#### 7.1 event参数

- 没有其它参数时调用不用写

```javascript
<template>
    <div>       
        <button @click="increment1">+1</button>     
    </div>
</template>
<script>
export default {
    methods: {
        increment1(event) {
            console.log(event.target)
            console.log(event.currentTarget) 
            console.log('event', event, event.__proto__.constructor) 
            this.num++
        },       
}
</script>
```

- 有其他参数时调用写成$event

```javascript
<template>
    <div>        
        <button @click="increment2(2, $event)">+2</button>
    </div>
</template>
<script>
export default {
increment2(val, event) {           
            console.log(event.target)
            this.num = this.num + val
        },
 }
</script>      
```

- event是原生事件
```javascript
console.log('event', event, event.__proto__.constructor) 
// 是原生的 event 对象
```
- event事件被挂载到当前元素，和 React 不一样
```javascript
console.log(event.currentTarget) 
// 事件是被注册到绑定事件的元素，而不是触发事件的元素
```
#### 7.2  一个事件调用多个函数
<font size=3>&emsp;&emsp;写成执行方式，用逗号分隔
```javascript
<button @click="handerBtn1Click(),handerBtn2Click()" > 
	button 
</button >
```

#### 7.3  修饰符

- 用法
	- v-on:事件名称.修饰符="执行函数或表达式"
	- v-on:可简写成@

- 事件修饰符

	- stop

		-   阻止事件冒泡

	- prevent

		- 阻止默认事件

	- self

		- 只当event.target是自己时执行

	- once

		- 只触发一次

	- capture

		- 事件的运行变成捕获模式

	- passive 

		- 滚动事件的默认行为 (即滚动行为) 将会立即触发，而不会等待 onScroll 完成

- 修饰符串联
	- v-on:click.prevent.self 会阻止所有点击事件
	- v-on:click.self.prevent 只会阻止自身点击

- 按键修饰符
```javascript
<input @keydown.enter="handerkeydown" />
	//只有按enter键才会触发
<button v-on:click.ctrl="handleclick">
	//click+Ctrl或Alt+click+Ctrl 或 Shift+click+Ctrl 被一同按下时触发
<button v-on:click.ctrl.exact="handleclick">
	//只有click+Ctrl会触发
<button v-on:click.exact="handleclick">
	//只有click会触发
```

## 8、表单 v-model双向数据绑定


#### 8.1 输入框
 - input通过v-model实现与 data中数据的双向绑定
 - v-model的值代替value，无需写value
```javascript
<input type="text" v-model.trim="name"/>
```
	
#### 8.2 多行文本
 - 用法与input类似
```javascript
 <textarea v-model="desc"></textarea>
 //<textarea>{{desc}}</textarea> 是不允许的
```

#### 8.3 复选框

 - checked需为布尔值

```javascript
<input type="checkbox" v-model="checked"/>
```
#### 7.4 多个复选框

 - value赋值，同时data中定义checkedNames为空数组  
 - 复选时，value值添加到数组checkedNames中

```javascript
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">       
<input type="checkbox" id="john" value="John" v-model="checkedNames">
```

#### 8.5  radio单选

 - 原理与多个复选框类似，只是因为单选，gender需定义成字符串

```javascript
<input type="radio" id="male" value="male" v-model="gender"/>
 <label for="male">男</label>
<input type="radio" id="female" value="female" v-model="gender"/>
<label for="female">女</label>
```
#### 8.6 下拉列表单选

 - 原理同radio,data中定义selected为空字符串

```javascript
 <select v-model="selected">
            <option disabled value="">请选择</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
</select>
```
 

#### 8.7 下拉列表多选

 - 原理同多个复选框，selectedList定义为空数组

```javascript
 <select v-model="selectedList" multiple>
            <option disabled value="">请选择</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
  </select>
```

#### 8.8 表单修饰符

- trim：去掉输入的首尾空格	

```javascript
 <input type="text" v-model.trim="name"/>
```

- lazy：表单失去焦点后再显示在页面上
```javascript
<input type="text" v-model.lazy="name"/>
```

- number： 改变数据类型为number		 

```javascript
<input type="text" v-model.number="age"/>
```
<font size=3>表单代码：
```javascript
<template>
    <div>
        <p>输入框: {{name}}</p>
        <input type="text" v-model.trim="name"/>
        <input type="text" v-model.lazy="name"/>
        <input type="text" v-model.number="age"/>

        <p>多行文本: {{desc}}</p>
        <textarea v-model="desc"></textarea>
        <!-- 注意，<textarea>{{desc}}</textarea> 是不允许的！！！ -->

        <p>复选框 {{checked}}</p>
        <input type="checkbox" v-model="checked"/>

        <p>多个复选框 {{checkedNames}}</p>
        <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
        <label for="jack">Jack</label>
        <input type="checkbox" id="john" value="John" v-model="checkedNames">
        <label for="john">John</label>
        <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
        <label for="mike">Mike</label>

        <p>单选 {{gender}}</p>
        <input type="radio" id="male" value="male" v-model="gender"/>
        <label for="male">男</label>
        <input type="radio" id="female" value="female" v-model="gender"/>
        <label for="female">女</label>

        <p>下拉列表选择 {{selected}}</p>
        <select v-model="selected">
            <option disabled value="">请选择</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
        </select>

        <p>下拉列表选择（多选） {{selectedList}}</p>
        <select v-model="selectedList" multiple>
            <option disabled value="">请选择</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
        </select>
    </div>
</template>

<script>
export default {
    data() {
        return {            
            desc: '自我介绍',

            checked: true,
            checkedNames: [],

            gender: 'male',

            selected: '',
            selectedList: []
        }
    }
}
</script>
```


## 9、思维导图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210610140337136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODY0NDM5Nw==,size_16,color_FFFFFF,t_70#pic_center)

