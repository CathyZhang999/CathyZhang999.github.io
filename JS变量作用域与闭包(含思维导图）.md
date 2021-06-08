@[toc]
## 一、变量的作用域

### 1、全局作用域
- 不定义在任何函数内部的变量
- 在函数内不使用var声明的变量

&emsp;&emsp;<font size=3>没有使用var声明的变量，在方法内部或外部都是全局变量，但如果是在方法内部声明，在方法外部使用之前需要先调用方法，告知系统声明了全局变量后方可在方法外部使用。
  
  

### 2、函数作用域

 - <font size=4>**在函数内使用var声明的变量** </font>

<font size=3>&emsp;&emsp;注意：不加var定义的是全局变量	 

```javascript
<script>
        var a = 1;
        var b = 2;
        function fun() {
            // 字母c没有加var关键字定义，所以它就变为了全局变量
            c = 3;
            c++;
            var b = 4;
            b++;
            console.log(b); // 5
        }
        fun();
        console.log(b); // 2
        // 在函数外部，是可以访问变量c的
        console.log(c); // 4
 </script>
```
	  

 - <font size=4> **形参也是局部变量**
```javascript
      var a=10;
	  function fun(a){
	      a++;
	      console.log(a);  
	  }
	  fun (7)                 //输出8
	  console.log(a);         //输出10
```	  
 - <font size=4> **注意变量的声明提升**

```javascript
   <script>
        var m = 1;
        function fun() {
            m++;
            //2、m变量声明提升后值为undefined，加1后为NaN
            var m = 4;
            //1、变量m的声明会提升到函数内最前面，但值不会提升
            console.log(m);
            //3、前一条语句将m赋值为4 ，因此，此时 输出4	
        }
        fun();
        console.log(m); //输出 1
    </script>
```

	  

### 3、块级作用域

- 作用范围：用{}包裹起来的代码块
- 变量定义：用const、let定义的变量，var定义的变量没有块级作用域
- 语句范围：for、while(do while)、if、switch

## 二、变量的作用域链

### 1、自由变量
<font size=3>&emsp;&emsp;变量在当前作用域未定义而被使用

### 2、变量的查找原则
 - 自由变量会向上级作用域逐层寻找它的定义，直到全局作用域;
 - 若全局作用域也未找到，则为报错：xxx is undefined.
```javascript
<script>
        var a = 10;
        var b = 20;
        function fun() {
            var c = 30;
            function inner() {
                var a = 40;
                var d = 50;
                console.log(a, b, c, d);
            }
            inner();
        }
        fun();        
    </script>
```


## 三、闭包（closure)

### 1、定义
<font size=3>&emsp;&emsp;函数本身和函数声明时所处的环境状态的组合

### 2、原理
<font size=3>&emsp;&emsp;自由变量在函数定义的地方向上查找，而不是函数执行的地方</font>

### 3、表现

 - <font size=4> **函数作为参数被传递**

```javascript
 function print(fn) {
      const a = 200
      fn()
  }
  const a = 100
  function fn() {
      console.log(a)
  }
  print(fn)    // 100  在函数定义的地方查找 
```

 - <font size=4> **函数作为返回值被返回**

```javascript
  function create() {
      const a = 100
      return function () {
          console.log(a)
      }
  }
  const fn = create()       //等于将内部的函数传给了fn
  const a = 200
  fn() // 100               //执行内部函数     
```

 - <font size=4> **立即执行函数（IIFE）**

```javascript
var arr = [];
for (var i = 0; i < 5; i++) {
    (function (i) {
        arr.push(function () {
            console.log(i);
        });
    })(i);
}
arr[0](); //0       
arr[3](); //3
/* 立即执行函数形成了闭包，每次执行后都会记住变量i的值，
因此会输出0和4，否则会全部输出5。 */
```

### 4、使用场景

- <font size=4> **模拟私有变量**

```javascript
  <script>
          // 封装一个函数，这个函数的功能就是私有化变量
          function fun() {
          // 定义一个局部变量a
              var a = 0;  
              return {
                  getA: function () {
                      return a;
                  },
                  add: function () {
                      a++;
                  },
                  pow: function () {
                      a *= 2;
                  }
              };
          }
          var obj = fun();
          // 如果想在fun函数外面使用变量a，唯一的方法就是调用getA()方法
          console.log(obj.getA());      //0
          // 若让变量a进行加1操作
          obj.add();  
          obj.add();
          obj.add();
          console.log(obj.getA());     //3
          obj.pow();
          console.log(obj.getA());     //6
      </script>
```

- <font size=4> **延长变量的生命周期（记忆性）**

<font size=3>&emsp;&emsp;函数所处环境的状态会始终保持在内存中，不会在外层函数调用后被自动清除。这就是闭包的记忆性。
 

```javascript
 <script>
       function createCheckTemp(standardTemp) {
            function checkTemp(n) {
                if (n <= standardTemp) {
                    alert('你的体温正常');
                } else {
                    alert('你的体温偏高');
                }
            }
            return checkTemp;
        }
        
        // 创建一个checkTemp函数，它以37.1度为标准线
        var checkTemp_A = createCheckTemp(37.1);
        // 再创建一个checkTemp函数，它以37.3度为标准线
        var checkTemp_B = createCheckTemp(37.3);

        checkTemp_A(37.2);
        checkTemp_A(37.0);
        checkTemp_B(37.2);
        checkTemp_B(37.0);
 </script>
```

### 5、注意事项

<font size=3>&emsp;&emsp;不要滥用闭包。  

 - <font size=3>避免造成网页性能问题
 <font size=3>&emsp;&emsp;因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响

 - <font size=3>避免造成内存泄露
  <font size=3>&emsp;&emsp;内存泄漏：程序中动态分配的内存由于某种原因未被释放或无法释放

### 5、思维导图                     
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606133959296.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODY0NDM5Nw==,size_16,color_FFFFFF,t_70#pic_center)
                     	  

