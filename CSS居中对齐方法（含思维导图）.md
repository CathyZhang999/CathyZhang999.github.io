- [absolute与relative](#absolute与relative)
- [居中对齐方法](#居中对齐方法)
  - [inline元素](#inline元素)
  - [block元素](#block元素)
  - [absolute元素](#absolute元素)
  - [Flex 布局](#flex-布局)
- [思维导图](#思维导图)
## absolute与relative
 1. <font size=4>relative 
<font size=3>&emsp;&emsp;相对定位，基于自身定位
 2. <font size=4>absolute
<font size=3>&emsp;&emsp;绝对定位，基于最近的定位元素定位
&emsp;&emsp;&emsp;  - 定位元素1：absolute relative fixed
&emsp;&emsp;&emsp; - 定位元素2：body

## 居中对齐方法

### inline元素

- 水平居中
 text-align:center

- 垂直居中
	line-height等于height值

```javascript
 .span1{
 			text-align: center;
            line-height: 200px;
            height: 200px;
        } 
```

 
### block元素

- 水平居中
	margin:auto

### absolute元素
- 方案一
利用margin负值，需知道宽、高。
	- 水平：left:50% margin-left:负宽度一半
	- 垂直：top:50% margin-top:负高度一半
	

```javascript
.box1{
			width: 300px;
            height: 100px;
            position: absolute;
            left: 50%;
            margin-left: -150px;
            top: 50%;
            margin-top: -50px;
      }
```

 - 方案二
利用transform移动，无需知道宽高。
	- 水平：left:50%  transform -50%
	- 垂直：top:50%  transform -50%
```javascript
.box2{
			position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%)      
	}
```

 - 方案三

	- top left bottom right :0
	- margin:auto
```javascript
.box3{
			position: absolute;
            top: 0;
            left: 0;
            bottom: 0;
            right: 0;
            margin: auto;    
	}              
```
### Flex 布局


 <font size=3>说明：在父元素上设置，在子元素上生效。 
 
 - 主轴居中对齐
 justify-content:center
 - 交叉轴居中对齐
align-items:center

```javascript
//CSS部分
.container{
            display: flex;
            justify-content: center;
            align-items: center;
            width: 1000px;
            height: 600px;
        }
.box1{
            width: 100px;
            height: 100px;
            background-color: blue; 
 }

//DOM结构
<div class="container">
        <div class='box1'>
        </div>
</div> 

 
```
## 思维导图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606152026797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODY0NDM5Nw==,size_16,color_FFFFFF,t_70#pic_center)

