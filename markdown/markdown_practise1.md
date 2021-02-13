<div align="center" style="color:red">VS Code 使用 Markdown 编写文档</div>  

<span id = "manu">目录</span>

[1. 直接使用HTML标签，可以设置文字居中，字体颜色等样式(HTML语法)](#1-直接使用html标签可以设置文字居中字体颜色等样式html语法)  
[2. 标题](#2-标题)  
[3. TOC(根据标题生成目录)](#3-toc根据标题生成目录)  
[4. 引用](#4-引用)  
[5. 行内标记(用 \` 标记代码块将变成一行)](#5-行内标记用-\-标记代码块将变成一行)  
[6. 代码块](#6-代码块)  
[7. 插入链接](#7-插入链接)  
[8. 插入图片](#8-插入图片)  
[9. 插入视频](#9-插入视频)  
[10. 序列](#10-序列)  
[11. 任务列表(类似于多选框)](#11-任务列表类似于多选框)  
[12. 表格](#12-表格)  
[13. 支持内嵌CSS样式](#13-支持内嵌css样式)  
[14. 语义标记](#14-语义标记)  
[15. 语义标签](#15-语义标签)  
[16. 公式](#16-公式)  
[17. 分隔符](#17-分隔符)  
[18. 脚注](#18-脚注)  
[19. 锚点](#19-锚点)  
[20.  自动邮箱链接](#20--自动邮箱链接)

### 1. 直接使用HTML标签，可以设置文字居中，字体颜色等样式(HTML语法)  

```html
<div align="center" style="color:red">VS Code 使用 Markdown 编写文档</div>  
```

### 2. 标题

***注：“#”后面有一个空格（和使用h1/h2标签功能类似）***

### 3. TOC(根据标题生成目录)

- [h1](#h1)
- [h2](#h2)

### 4. 引用

代码1(单行式)：  
>hello world!  

代码2(多行式)：  
>hello world!  输入完之后按两次空格键再按一次Enter键即可
>hello world!  输入完之后按两次空格键再按一次Enter键即可
hello world!  输入完之后按两次空格键再按一次Enter键即可
hello world!  

代码3(多层嵌套)：
>aaaaaaa  
>>bbbbbbb  
>>>cccccc  

### 5. 行内标记(用 \` 标记代码块将变成一行)

   代码：标记之外`hello world`标记之外  

### 6. 代码块

#### 用\`\`\`代码\`\`\`进行包裹

```
<div>
<div></div>
</div>
```

#### 自定义语法（根据不同的语言配置不同的代码着色）

```javascript
var num = 0
for (var i = 0; i < 5; i++>){
    num+=i;
}
console.log(num);
```

### 7. 插入链接

[百度](http://www.baidu.com/ '百度一下')

### 8. 插入图片

 1. 插入本地图片
    ![small](small.jpg '笑脸')
    ![笑脸](small.jpg)
 2. 插入网络图片
    ![rollbot.png](https://i.loli.net/2019/10/18/si38LmGjAUtdF6T.png '笑脸')

### 9. 插入视频

>Markdown 语法是不支持直接插入视频的  
>普遍的做法是 插入 HTML 的 iframe 框架，可以通过网站自带的分享功能获取  

```html
# 代码如下
<iframe height=498 width=510 src='http://player.youku.com/embed/XMjgzNzM0NTYxNg==' frameborder=0 'allowfullscreen'></iframe>
```

<iframe src="https://player.bilibili.com/player.html?aid=756070508&bvid=BV1Wr4y1T7K6&cid=279787964&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

### 10. 序列

#### 有序序列

 1. one
 2. two
 3. three

#### 无序序列

- one
- two
- three

#### 序表嵌套代码块

- one

    var a = 10 // 与上行保持空行并 递进缩进

### 11. 任务列表(类似于多选框)

- [ ]  选项一
- [x]  选项二

### 12. 表格

| a  | b  | c  |
|:--:|:-- | --:|
| 居中 | 左对齐 | 右对齐 |

| a  | b  | c  |
|:--:|:--:|:--:|
| 居中 | 居中 | 居中 |

a  |  b  | c
:---:|:------------ |--:
居中 |  左对齐 |  右对齐

### 13. 支持内嵌CSS样式  
  
<p style="color: #AD5D0F;font-size: 30px; font-family: '宋体';">内联样式</p>

### 14. 语义标记  

*斜体*、_斜体_  
**加粗**  
***加粗+斜体***、**_加粗+斜体_**  
~~删除线~~  
==背景色==  
$\underline{下划线}$  

### 15. 语义标签  

<i>斜体</i>  
<b>加粗</b>  
<em>强调</em>  
<u>下划线</u>  
<del>删除</del>  
Z<sup>a</sup>  
z<sub>a</sub>  
<kbd>Ctrl</kbd>

### 16. 公式  

$$ x \href{why-equal.html}{=} y^2 + 1 $$
$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a}. $$

### 17. 分隔符  

***

---

### 18. 脚注  

### 19. 锚点  

```
代码：
[公式标题锚点](#1)
#### [任务列表(类似于多选框)]{#1}
//见文章开头的目录
```

### 20.  自动邮箱链接  

<xx@outlook.com>

[返回目录](#manu)
