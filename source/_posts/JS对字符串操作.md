---
title: JS对字符串操作
date: 2017-03-02 18:44:08
categories: Font-end
tags: JavaScript
---

#### 截取字符串：

**1.split： 把一个字符串分割成字符串数组。**

*功能* ：使用一个指定的分隔符把字符串分隔存储到数组
*语法* ：<font color=red>str.split(separator,size)</font>
*参数* ：
- str：必选项。要截取的字符串。
- separator: 必选项。要分割的条件，是字符串或表达式。
- size：可选项。返回数组的长度。不定义则全部返回。

*实例* ：
``` javascript
var str=”jpg|bmp|gif|ico|png”; arr=str.split(”|”);//arr是一个包含字符值”jpg”、”bmp”、”gif”、”ico”和”png”的数组
```
<!--more-->

**2.slice：   提取字符串的某个部分。**

*功能*：返回一个新的数组，包含从start到end（不包括该元素）的arrayobject中的元素。
*语法*：<font color=red>str.slice(startPos,endPos)</font>
*参数*：
- startPos: 必选项。字符串的起始位置。如果参数负数，则从字符串的结尾处算起。 也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。
- endPos: 可选项。提取字符串的结束小标。 如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。

*实例*：
``` javascript
var str='ahji3o3s4e6p8a0sdewqdasj'
alert(str.slice(2,5))   //结果ji3
```


**3.substring：  返回指定位置的子字符串。**

*功能*：用于提取字符串中介于两个指定下标之间的字符。
*语法*：<font color=red>str.substring(startPos,endPos)</font>
*参数*：
- str: 必选项。要提取的字符串。
- startPos: 必选项。子字符串的起始位置，该索引从0开始计算。 一个非负的整数，规定要提取的子串的第一个字符在 stringObject 中的位置。
- endPos: 可选项。子字符串的结束位置，该索引从0开始计算。 一个非负的整数，比要提取的子串的最后一个字符在 stringObject 中的位置多 1。 如果省略该参数，那么返回的子串会一直到字符串的结尾。

> 注： 返回 一个新的字符串，该字符串值包含 stringObject 的一个子字符串，其内容是从 start 处到 stop-1 处的所有字符，其长度为 stop 减 start。 说明 substring 方法返回的子串包括 start 处的字符，但不包括 end 处的字符。 如果 start 与 end 相等，那么该方法返回的就是一个空串（即长度为 0 的字符串）。 如果 start 比 end 大，那么该方法在提取子串之前会先交换这两个参数。 如果 start 或 end 为负数，那么它将被替换为 0。

*实例*：
``` javascript
str='ahji3o3s4e6p8a0sdewqdasj'
alert(str.substring(2,6))   //结果为ji3o3
```


**4.substr:    返回字符串中指定位置开始的指定长度的子字符串。**

*语法*：<font color=red>str.substr(startPos,length)</font>
*参数*：
- str：必选项。要提取的字符串。
- startPos: 必选项。返回字符串的起始位置，单位为数字。 字符串中的第一个字符的索引为 0。为负数，则默认为0
- length: 可选项。返回字符串的字符个数。 说明 如果 length 为 0 或负数，将返回一个空字符串。 如果没有指定该参数，则子字符串将延续到stringObject的最后。

*实例*：
``` javascript
var str = "0123456789";
alert(str.substring(5));	//"56789"
alert(str.substring(10));	//""
alert(str.substring(2,12));	//"23456789"
alert(str.substring(2,-2));	//"01"
alert(str.substring(-1,5));	//"01234"
```



#### 合并字符串

**1.join**

*功能*：使用您选择的分隔符将一个数组合并为一个字符串。

*实例*：
``` javascript
var delimitedString=myArray.join(delimiter);
var myList=new Array(”jpg”,”bmp”,”gif”,”ico”,”png”);
var portableList=myList.join(”|”);//结果是jpg|bmp|gif|ico|png
```
 
**2.concat**

*功能*：将两个数组连接在一起。

*实例*：
``` javascript
arr1=[1,2,3,4]
arr2=[5,6,7,8]
alert(arr1.concat(arr2))  //结果为[1,2,3,4,5,6,7,8]
```

#### 其他：

**1.charAt**

 *功能*：返回指定位置的<font color=red>字符</font>。字符串中第一个字符的下标是 0。如果参数 index 不在 0 与 string.length 之间，该方法将返回一个空字符串。

*实例*:
``` javascript
var str='a,g,i,d,o,v,w,d,k,p'
alert(str.charAt(2))  //结果为g
```

**2.charCodeAt**

*功能*：charCodeAt() 方法可返回指定位置的字符的<font color=red>Unicode 编码</font>。这个返回值是 0 - 65535 之间的整数。
方法 charCodeAt() 与 charAt() 方法执行的操作相似，只不过前者返回的是位于指定位置的字符的编码，而后者返回的是字符子串。

*实例*：
``` javascript
var str='a,g,i,d,o,v,w,d,k,p'
alert(str.charCodeAt(2))  //结果为103。即g的Unicode编码为103
```

**3.replace:   用于在字符串中用一些字符替换另一些字符。**

*语法*：<font color=red>str.replace(string,replacement)</font>
*参数*：
- str：必选项。要替换的字符串。
- string:必选项。正则对象。
- replacement: 必选项。要替换的字符。

----记录这些是因为某些时候会记错某个方法返回结果，基本用法还是要记一记的，羞愧= =---- 