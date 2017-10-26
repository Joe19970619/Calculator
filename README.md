# JavaScript写个简易计算器

这个小练手旨在帮助刚**上手学习JavaScript的同学**练习最基本的Js知识，大神勿喷哈，由于这个是非常简易的计算器，可以完成的功能有加减乘除和AC（清屏），DEL（退格）等基本运算，所以代码也不复杂，**我会先放出代码，然后再讲解我的思路**，非常简单，只要你按照我的思路走，15分钟不到我保证你也可以敲出同样的代码，当然主要还是希望可以为入门的同学提供一种解决问题的思路，各位看完后可以自己动手敲一遍哈。

<!-- more -->

#### HTML代码部分
```JavaScript
 <table>
      <tr>
          <td colspan="4"><input class="txt" type="text" disabled/></td>
      </tr>
      <tr>
          <td colspan="2"><input class="btn_click" type="button" value="AC"/></td>
          <td colspan="2"><input class="btn_click"  type="button" value="DEL"/></td>
      </tr>
      <tr>
          <td><input class="btn" type="button" value="7"/></td>
          <td><input class="btn" type="button" value="8"/></td>
          <td><input class="btn" type="button" value="9"/></td>
          <td><input class="btn" type="button" value="*"/></td>

      </tr>
      <tr>
          <td><input class="btn" type="button" value="4"/></td>
          <td><input class="btn" type="button" value="5"/></td>
          <td><input class="btn" type="button" value="6"/></td>
          <td><input class="btn" type="button" value="/"/></td>

      </tr>
      <tr>
          <td><input class="btn" type="button" value="1"/></td>
          <td><input class="btn" type="button" value="2"/></td>
          <td><input class="btn" type="button" value="3"/></td>
          <td><input class="btn" type="button" value="-"/></td>

      </tr>
      <tr>
          <td><input class="btn" type="button" value="0"/></td>
          <td><input class="btn" type="button" value="."/></td>
          <td><input class="btn" type="button" value="+"/></td>
          <td><input class="btn" type="button" value="="/></td>

      </tr>

</table>
```
HTML这部分非常简单，没什么多说的，整个框架我利用table搭建的，需要注意的是，由于计算器屏幕不可输入，我设置为了disabled。

#### CSS代码部分

```JavaScript
<style>
        table{
            border-collapse: collapse;
            margin: auto auto;
        }
        td{
            width: 150px;
            line-height: 70px;
        }
        .btn{
            width: 150px;
            line-height: 70px;
            font-size: x-large;
        }
        .btn_click{
            width: 302px;
            line-height: 70px;
            font-size: x-large;
        }
        .txt{
            width: 600px;
            line-height: 100px;
            font-size: x-large;text-align: right;
        }
    </style>
```
CSS也是简单设定了一下宽高，我是在火狐浏览器上运行的，由于我没使用百分比来设置宽高，在其他浏览器上样式会发生一定程度上的改变，不过不影响功能就是了。

#### JavaScript部分
**请先不要直接看这部分代码**，先看我的思路讲解再看这部分，你绝对可以轻松理解
```JavaScript
<script>
    /*在网页加载时  给按钮添加点击事件*/
    window.onload = function () {
        //定义数组  来接收用户按的数字和计算符号
        var way_res = [];
        //获取按钮对象
        var btn_txt = document.getElementsByClassName("btn");
        //获取屏幕元素
        var txt = document.getElementsByClassName("txt")[0];
        //获取清空按钮和退格按钮
        var btn_way = document.getElementsByClassName("btn_click");
        for (var i = 0; i < btn_way.length; i++) {
            btn_way[i].onclick = function () {
                //判断按钮
                if (this.value == "AC") {
                    way_res = [];
                    txt.value = "";
                }
                else {
                    /* substr() 截断字符串 1.从那个位置开始   2.截取多少长度*/
                    txt.value = txt.value.substr(0, txt.value.length - 1);
                }
            }
        }
        //给btn_txt  数组对象添加事件
        for (var i = 0; i < btn_txt.length; i++) {
            btn_txt[i].onclick = function () {
                /* this 指代的是当前事件的执行对象*/
                /*按完键将值传给屏幕*/
                /*判断是否为数字*/
                if (txt.value == "" && this.value == ".") {
                    txt.value = "0.";
                }
                else {
                    if (!isNaN(this.value) || this.value == ".") {
                        /*用户输入的是数字或者点的情况*/
                        /*indexOf() 用来查找字符  如果有返回当前位置  如果没有返回-1*/
                        if (txt.value.indexOf(".") != -1) {
                            /*有点存在的情况*/
                            if (this.value != ".") {
                                /*当前按得不是点，进行拼接*/
                                txt.value += this.value;
                            }
                        }
                        else {
                            /*没点存在直接拼接*/
                            txt.value += this.value;
                        }
                    }
                    else {
                        /*是符号的情况*/
                        //先存值  在清屏
                        if (this.value != "=") {
                            /*是符号但不为等号的情况*/
                            way_res[way_res.length] = txt.value;
                            //存符号
                            way_res[way_res.length] = this.value;
                            //清屏
                            txt.value = "";
                        }
                        else {
                            /*是等号的情况*/
                            way_res[way_res.length] = txt.value;
                            //eval()方法   专门用来计算表达式的值
                            txt.value = eval(way_res.join(""));
                            //计算完成之后将数组清空
                            way_res = [];
                        }
                    }
                }
            }
        }
    }
</script>
```

### 思路讲解
请先不要看上面的JS代码，接下来请试着跟着我的思路走，完成这个计算器的功能，我是分成**三个部分**来解决的，**第一部分**是将除了AC，DEL这两个键之外的按键值获取到屏幕上，**第二部分**是计算屏幕上的表达式的值，**第三部分**是添加AC（清屏），DEL（退格）功能，检查BUG。

![](http://upload-images.jianshu.io/upload_images/2768522-ac4cafd1c7d4e06b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ##### 第一部分：获取值到屏幕上

我认为解决简单的JS问题大体都可以分三步：
- 1.获取你想操作的元素；
- 2.保存你想操作的元素；
- 3.对元素进行（遍历）操作；

我的第一步目的是将除了AC，DEL这两个键之外的按键值获取到屏幕上，那么我首先得**获取**按钮元素以及屏幕元素并将它们**保存**在btn_txt和txt变量里：
```JavaScript
    //获取按钮对象
    var btn_txt = document.getElementsByClassName("btn");
    //获取屏幕元素
    var txt = document.getElementsByClassName("txt")[0];
```
获取并保存了想操作的元素，接下来就对他们添加操作：
```JavaScript
  //给btn_txt  数组对象添加onclick事件
  for (var i = 0; i < btn_txt.length; i++) {
      btn_txt[i].onclick = function () {
}
```

![](http://upload-images.jianshu.io/upload_images/2768522-da54f56c6270c100.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在进行操作的之前请等一下，我们思考一下，btn_txt数组里存放着0,1,2,3,4,5,6,7,8,9，" . "，" + "，" - "，" * "，" \ "，" = "等一系列东西，我们当然要对数字和计算符号进行分开操作，所以我们用If……else……来判断一下
```JavaScript

 if (!isNaN(this.value) || this.value == ".") {
            /*用户输入的是数字或者点的情况*/
           /* this 指代的是当前事件的执行对象*/
}
 else{
           /*用户输入的是符号的情况*/
}
```
OK，我们接下来先考虑**用户输入的是数字或者点的情况**，首先数字可以连续输入到屏幕里，但是小数点不应该能连续输入到屏幕里，小数点应该只有一个才对，所以我们应该先加一个判断条件：屏幕里是否有小数点存在？如果屏幕里已经有小数点存在，那么我只允许你再输入数字，否则屏幕值不会接收，即是如下代码：
```JavaScript

 if (!isNaN(this.value) || this.value == ".") {
         /*用户输入的是数字或者点的情况*/
         /* this 指代的是当前事件的执行对象*/

         /*有点存在的情况*/
      if (txt.value.indexOf(".") != -1) {
      /*indexOf() 用来查找字符  如果有 返回当前位置  如果没有 返回-1*/
            if (this.value != ".") {
            /*当前按得不是点，进行拼接*/
            txt.value += this.value;
            /*这里的txt.value += this.value;意思是
            将当前用户所按的数字添加到屏幕上去*/
           }
   }
            else {
           /*没点存在直接拼接*/
                  txt.value += this.value;
           }
  }

else{
           /*用户输入的是符号的情况接下来再考虑*/
}
```
好了，用户输入的是数字或者点的情况已经考虑结束了，现在**考虑用户输入的是运算符号的情况**！这种情况也分两部分，一种是用户按了等号，一种是按了除等号之外的其他加减乘除运算符号，因为等号比较特殊一点，按了就直接应该得出结果了，所以要区用if……else……分开。我这样的思路你可以理解吧！

还有一个事情我们要考虑的是，我希望在我按下加减乘除运算符号时可以清屏，这样我就可以继续键入下一数字了（举例：我按下数字“5”，再按下运算符“ + ”，按下瞬间屏幕清屏，然后我再键入数字“3”，最后按下“ = ”，最后得出结果“2”），但是清屏我并不想让我的数据丢失，所以此时我先新建一个数组来保存这些数据（这里的“数据”指数字和运算符，也叫“表达式”），然后再清屏！具体代码如下：
```JavaScript
 var way_res = [];
/*先自定义一个数组  来接收用户按的数字和计算符号
注意这里定义要在开头定义，而不是在函数内部*/

if (!isNaN(this.value) || this.value == ".") {
         /*用户输入的是数字或者点的情况*/
        /*上面已经讨论过了，这里我省点地方*/
}

else{
    /*用户输入的是符号的情况*/
    //先存值  在清屏
    if (this.value != "=") {
       /*是符号但不为等号的情况*/
       way_res[way_res.length] = txt.value;
        //存符号
       way_res[way_res.length] = this.value;
        //清屏
       txt.value = "";
   }
    else{
      /*为等号的情况下面讨论*/
  }
}
```
- ##### 第二部分：计算屏幕上的表达式的值

好了下面我们讨论用户按下等号键的情况，这种比较简单，直接对表达式（表达式就是我们之前输入的数字与符号组合）进行计算就可以啦，需要注意的是计算完成之后要把保存表达式的数组way_res清空，因为本次运算完满结束了，如果不清空里面的数据会影响下一次正常计算；
```JavaScript
else {
       /*为等号的情况即是计算屏幕上表达式的值*/

       //先将屏幕上所有值保存到数组里
       way_res[way_res.length] = txt.value;
       //eval()方法   专门用来计算表达式的值
       //join()方法   将数组的每位拼接成字符串
       txt.value = eval(way_res.join(""));
       //计算完成之后将数组清空
       way_res = [];
 }
```

- ##### 第三部分：添加AC，DEL功能，检查BUG

首先，获取清空按钮和退格按钮，然后把它们保存在btn_way变量下；
然后就遍历进行添加功能，这里同样需要一个if……else……语句来判断用户按的是AC按钮还是DEL按钮
```JavaScript
var btn_way = document.getElementsByClassName("btn_click");
//获取清空按钮和退格按钮
 for (var i = 0; i < btn_way.length; i++) {
     btn_way[i].onclick = function () {
     //判断按钮
     if (this.value == "AC") {
     //如果是AC按钮，那么清空屏幕和数组里的一切值
         way_res = [];
         txt.value = "";
    }
     else {
     //不是AC，那必然是DEL，我们把屏幕的最后一位抹去再重新赋给屏幕

     /* substr(参数1，参数2) 截断字符串
     参数1.从那个位置开始   参数2.截取多少长度*/
         txt.value = txt.value.substr(0, txt.value.length - 1);
    }
   }
}
```
到这里为止，所有功能基本上全部添加完毕，然后我们进行调试，发现一个问题，就是当我们第一次按键就按小数点“ . ”时，这时用户的本意应为“ 0. ”,意即用户是想输入小数的，但是懒得按“0”，直接按了小数点，所以我们应该加一个判断条件来帮助用户，直接按小数点成为有意义的行为，代码如下：
```JavaScript
if (txt.value == "" && this.value == ".") {
                    txt.value = "0.";
                }
```
好了，最后再加上window.onload = function(){}，代码，到这里就全部结束了；大家有不理解的地方欢迎在评论里问我；

#### 再放一遍JavaScript全代码
```JavaScript
<script>
    window.onload = function () {
        var way_res = [];
        var btn_txt = document.getElementsByClassName("btn");
        var txt = document.getElementsByClassName("txt")[0];
        var btn_way = document.getElementsByClassName("btn_click");
        for (var i = 0; i < btn_way.length; i++) {
            btn_way[i].onclick = function () {
                if (this.value == "AC") {
                    way_res = [];
                    txt.value = "";
                }
                else {
                    txt.value = txt.value.substr(0, txt.value.length - 1);
                }
            }
        }
        for (var i = 0; i < btn_txt.length; i++) {
            btn_txt[i].onclick = function () {
                if (txt.value == "" && this.value == ".") {
                    txt.value = "0.";
                }
                else {
                    if (!isNaN(this.value) || this.value == ".") {
                        if (txt.value.indexOf(".") != -1) {
                            if (this.value != ".") {
                                txt.value += this.value;
                            }
                        }
                        else {
                            txt.value += this.value;
                        }
                    }
                    else {
                        if (this.value != "=") {
                            way_res[way_res.length] = txt.value;
                            way_res[way_res.length] = this.value;
                            txt.value = "";
                        }
                        else {
                            way_res[way_res.length] = txt.value;
                            txt.value = eval(way_res.join(""));
                            way_res = [];
                        }
                    }
                }
            }
        }
    }
</script>
```
