> 参考资料：
>
> 《Java Web应用开发与实践(第2版)》 清华大学出版社
>
> 《MySQL必知必会》 人民邮电出版社
>
> 尚硅谷JavaWeb全套教程 BV1Y7411K7zz
>
> 黑马程序员最新版JavaWeb基础教程 BV1Qf4y1T7Hx
>
> [w3school 在线教程](https://www.w3school.com.cn/index.html)
>
> [菜鸟教程 - 学的不仅是技术，更是梦想！ (runoob.com)](https://www.runoob.com/)

# 【HTML5】

> **HTML：超文本标记语言**（ HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。
>
> [HTML 5 参考手册 (w3school.com.cn)](https://www.w3school.com.cn/html5/html5_reference.asp)

## 语法规范

### 基本语法概述

- HTML 标签是由**尖括号**包围的关键字词，例如：`<html>`。

- **双标签:** ` <html>` 和 `</html>`

- **按标签:** `<br/>`

- **结束符:** `/` 

  每个标签原则上都应该有**结束符**，单标签可以不加，**实际开发中建议不要给单标签添加斜线**。

- HTML中标签和属性名称对大小写并不敏感，但在代码中推荐使用小写，因为在XHTML中必须使用小写，所以任何标签都建议不要大写，即便是 `<!doctype html>` 标签。

- **注释格式：** `<!--内容-->`

### 标签关系

1. **包含关系**

```html
<head>
    <title>标题</title>
</head>
```

1. **并列关系**

```html
<head>
</head>
<body> 
</body>
```

## HTML基础

### HTML骨架

> HTML 页面也称为 HTML 文档。

【HTML5标准基础结构】:

```html
<!doctype html>
<html>
    <head>
        头部信息
        <title>标题</title>
    </head>
    <body>
        文档主体，正文部分
    </body>
</html>
```

|        标签名        |   定义   | 说明                                                        |
| :------------------: | :------: | ----------------------------------------------------------- |
|  `<html>` `</html>`  |  根标签  | `<html>`标签组表示标记间的内容是HTML文档                    |
|  `<head>` `</head>`  | 头部标签 | `<head>`标签组主要包括文档的头部信息，例如标题，可省略      |
| `<title>` `</title>` | 标题标签 | `<title>`标签组表示该HTML文档的标题（即浏览器标签页的内容） |
|  `<body>` `</body>`  | 主体标签 | `<body>`标签组内存放HTML文档的正文内容                      |

### 文档类型声明标签

```html
<!-- 当前页面采用 HTML5 版本 -->
<!doctype html>
```

`<!doctype>` 文档类型声明，作用是告诉浏览器应该使用哪种 HTML 版本来解析渲染网页，尽管不是必须的，但强烈建议添加

**注意：**

- `<!doctype>` 必须是HTML文档的第一行，在`<html>` 标签之前
- `<!doctype>` 文档类型声明标签，不属于 HTML 标签
- HTML5 版本 `<!doctype html>`
- HTML4 版本 `<!doctype dtd>`

### 标准骨架

vscode使用`!`(英文状态)生成标准骨架

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>网页标题</title>
</head>

<body>
    网页主体内容
</body>

</html>
```

## 常用标签

### 标签语义

**简单的理解：**标签的含义，即：这个标签是用来干嘛的

标签是用来分割和标记文本的元素，以形成文本的布局、文字的格式以及五彩缤纷的页面

### 注释

**场景：** 在 HTML 文档中添加一些便于阅读和理解，但又不需要显示在页面中的文本

**代码：**

HTML 中的注释以：`<!--` 开头，以 `-->` 结束。

```html
<!-- 注释语句 -->
```

> 注释是为了更好地解释代码功能，便于相关开发人员理解和阅读代码，程序是不会执行注释内容的。

### 标题标签

**场景：** 在新闻和文章的页面中，都离不开**标题**，用来突出显示文章主体

**代码：** HTML 提供了 6 个等级的网页标题，即：`<h1>` 到 `<h6>`

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
```

**语义：** 作为标题使用，重要程度依次递减。

> h是head的缩写

**特点：**

- 文字自动加粗
- 文字都有变大，从h1→h6逐渐变小
- 独占一行

**效果：**

![image-20220417173156659](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220417173156659.png)

### 段落标签

**场景：** 在新闻和文章的页面中，用于分段显示

**代码：** `<p></p>`标签

```html
<p>我是一个段落文字</p>
```

**语义：**可以把 HTML 文档分割为若干段落。

> p是paragraph的缩写

**特点：**

- 文本在一个段落中会根据浏览器窗口的大小自动换行

  中文段落一定会换行，连续的英文字母会被认为整体是一个单词，不会自动换行，要想英文段落自动换行，字母之间得有空格。

- 段落之间存在空隙（段间距）

- 段落独占一行（行间距）

![image-20220417174823317](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220417174823317.png)

### 换行标签

> 在 HTML 中，一个段落中的文字会从左到右依次排列，直到浏览器窗口的右端，然后才自动换行。
>
> 如果希望某段文本强制换行显示，就需要使用换行标签 `<br>`。

**场景：** 让文字强制换行显示

**代码：** `<br>`标签

```html
<br>
```

**语义：** 换行

> br是break的缩写

**特点：**

- 单标签
- 让文字强制换行
- 没有段间距

### 分割线标签

**场景：** 分割不同主题内容的水平线

**代码：** `<hr>`标签

```html
<hr>
```

**语义：** 主题的分割转换

> hr是horizon的缩写

**特点：**

- 单标签
- 在页面中显示一条水平线
- 实际开发中并不常用 hr 作为分割线，而是使用 CSS 盒子模型中的边框来实现分割线效果，或是利用一个空盒子设置长宽高及背景颜色来实现分割线效果。

### 文本格式化标签

**场景：** 在网页中，有时需要为文字设置粗体、斜体或下划线等效果，这时就需要用到 HTML 中的文本格式化标签，使文字以特殊的方式显示。

**代码：**

|  功能  |                  标签                  |
| :----: | :------------------------------------: |
|  加粗  | `<strong>` `</strong>` 或 `<b>` `</b>` |
|  倾斜  |     `<em>` `</em>` 或 `<i>` `</i>`     |
| 删除线 |    `<del>` `</del>` 或 `<s>` `</s>`    |
| 下划线 |    `<ins>` `</ins>` 或 `<u>` `</u>`    |

两种标签效果没有区别，如果语境中更重要推荐使用后者，语义更强烈

**语义：** 对页面元素进行强调

### 媒体标签

#### 图片标签

**场景：** 在网页中显示图片

**代码：** `<img src=" " alt=" ">`

```html
<img src="图像URL">
```

`src` 是 `<img>` 标签的必须属性，它用于指定图像文件的路径和文件名。

`URL` 是 `统一资源定位符`。

**语义：**

> img是image的缩写。

**特点：**

- 单标签
- img标签需要展示对应的效果，需要借助标签的属性进行设置
- 属性均采取`key="value"` 格式
- 图像标签可以存在多个属性
  - 属性之间没有顺序之分
  - 属性名之间以空格隔开
  - 标签名与属性名之间必须有空格隔开

**属性：**

|   属性   |  属性值  |                             说明                             |
| :------: | :------: | :----------------------------------------------------------: |
|  `src`   | 图片路径 |                           必须属性                           |
|  `alt`   |   文本   | 替换文本，图像显示失败时显示（为了提高 SEO 及适配屏幕阅读器，建议都把 alt 写上） |
| `title`  |   文本   |           提示文本，鼠标放到图片上，显示的提示文字           |
| `width`  |   像素   |             设置图像的宽度（一般通过 CSS 设置）              |
| `height` |   像素   |             设置图像的高度（一般通过 CSS 设置）              |
| `border` |   像素   |           设置图像的边框粗细（一般通过 CSS 设置）            |

- 设置图像的宽度与高度时：一般设置其中之一便可，另外一个会自动按比例适配
- 如果同时设置width和height两个，设置不当图片可能会变形
- 设置宽高时，可以使用百分数作为值，此时图片大小会以当前父元素的大小为基础进行比例缩放，这样做的好处在于当父元素改变大小时，图片也会随比例同等缩放

#### 路径

##### 相对路径

相对路径：从当前文件开始出发找目标文件的过程

| 相对路径分类 | 符号  |                  说明                  |
| :----------: | :---: | :------------------------------------: |
|  同一级路径  |  `.`  |    如：`<img src="./baidu.png" />`     |
|  下一级路径  |  `/`  |  如：`<img src="image/baidu.png" />`   |
|  上一级路径  | `../` | 如：`<img src="../image/baidu.png" />` |

##### 绝对路径

绝对路径：指目录下的绝对位置，直接到达目的位置，通常是从盘符开始的路径。

如 Windows 系统的绝对路径：`D:\web\img\test.png`

- 网络地址

  `https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/test.png`

**注意：**

- 相对路径为 `/`(正斜杠)
- windows绝对路径为 `\`(反斜杠)
- linux绝对路径用`/`，`\`作为转义字符
- 实际开发中建议使用相对路径或网络地址（都是 `/` 正斜杆）

#### 音频标签

**场景：** 在页面中插入音频

**代码：** 

```html
<audio src="xxx" controls></audio>
```

**常见属性：**

|  属性名  |            功能            |
| :------: | :------------------------: |
|   src    |         音频的路径         |
| controls |     显示播放音频的控件     |
| autoplay | 自动播放(部分浏览器不支持) |
|   loop   |          循环播放          |

**注意点：**

- 音频标签目前支持三种格式：MP3、Wav、Ogg

#### 视频标签

**场景：** 在页面中插入视频

**代码：**

```html
<video src="xxx" controls></video>
```

**属性名: **

|  属性名  |                   功能                   |
| :------: | :--------------------------------------: |
|   src    |                视频的路径                |
| controls |            显示播放视频的控件            |
| autoplay | 自动播放(谷歌浏览器中需静音才能自动播放) |
|   loop   |                 循环播放                 |
|  muted   |                 静音播放                 |

**注意点：**

- 视频标签目前支持三种格式：MP4、WebM、Ogg

### 超链接标签

**场景：** 点击之后，从一个页面跳转到另一个页面

**称呼：** a标签、超链接、锚链接

**代码：** 

```html
<a href="./目标网页.html" target="目标窗口的弹出方式">链接的显示效果</a>
```

> a是anchor的缩写，意为：锚。

**特点：**

- 双标签，内部可以包容内容

**属性：**

- `href`：用于指定链接目标的 url 地址，如果暂不知道跳转位置，使用空连接或者锚点链接

- `target`：用于指定链接页面的打开方式：

  |     值      |                  功能                  |
  | :---------: | :------------------------------------: |
  |   `_self`   |    覆盖原网页的方式跳转（为默认值）    |
  |  `_blank`   |        在新窗口中打开的方式跳转        |
  |  `_parent`  |    在框架窗口的父窗口打开被链接页面    |
  |   `_top`    |   在框架窗口的顶层窗口打开被链接网页   |
  | `framename` | 在指定的框架窗口的子窗口打开被链接网页 |

**链接分类：** 

- **外部链接：**例如：`<a href="http://www.baidu.com">百度</a>`

- **内部链接：**网站内部页面之间相互链接，直接链接内部页面名称即可，例如： `<a href="index.html">首页</a>`

- **空链接：**如果当时没有确定链接目标时，例如`<a href="javascript:void(0)">首页</a>"`，当用户点击链接时，void(0) 计算为0，但 Javascript 上没有任何效果

- **下载链接：**如果 href 里面地址是一个文件或者压缩包（前提：路径包含文件类型后缀名，如：`.exe`、`.zip` 等），便会下载这个文件

- **网页元素链接：**在网页中的各种网页元素，如 ：文本、图像、表格、音频、视频等都可以添加超链接

- **锚点链接：**点击链接，可以快速定位到当前页面中的某个位置

  - 定义锚点(使用id属性)：`<标签 id="锚点名字">`
  - 跳转到锚点: `<a href="#锚点名字">`

  - `<a href="#"></a>` 默认定位到页面顶部

### 列表标签

#### 应用场景

**场景：** 在网页中按照行展示关联性的内容，如：新闻列表、排行榜、账单等

**特点：** 按照行的方式，整齐显示内容

**种类：** 无序列表、有序列表、自定义列表

![image-20220419195621745](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220419195621745.png)

#### 无序列表

**场景：** 在网页中表示一组无顺序之分的列表，如：新闻列表

**标签组成：**

|   标签名    |                    说明                    |
| :---------: | :----------------------------------------: |
| `<ul></ul>` |     表示无序列表的整体，用于包裹li标签     |
| `<li></li>` | 表示无序列表的每一项，用于包含每一行的内容 |

> ul即unordered list
>
> li即list item

**显示特点：**

- 列表的每一项前默认显示圆点标识

**注意点：**

- `<ul>`标签只允许包含li标签
- `<li>`标签可以包含任意内容

**效果：**

```html
<body>
    <ul>
        <li>第一号</li>
        <li>第二号</li>
        <li>第三号</li>
        <li>
            <ul>
                <li>第2层1</li>
                <li>第2层2</li>
                <li>第2层3</li>
            </ul>
        </li>     
    </ul>
</body>
```

![image-20220419200340631](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220419200340631.png)

#### 有序列表

**场景：** 在网页中表示一组有顺序之分的列表，如：排行榜

**标签组成：** 

|   标签名    |                    说明                    |
| :---------: | :----------------------------------------: |
| `<ol></ol>` |     表示有序列表的整体，用于包裹li标签     |
| `<li></li>` | 表示有序列表的每一项，用于包含每一行的内容 |

> ol即ordered list
>
> li即list item

**显示特点：**

- `<ol>`标签只允许包含li标签
- `<li>`标签可以包含任意标签

**效果：**

```html
<body>
    <ol>
        <li>第一号</li>
        <li>第二号</li>
        <li>第三号</li>
        <li>
            <ol>
                <li>第2层1</li>
                <li>第2层2</li>
                <li>第2层3</li>
            </ol>
        </li>
    </ol>
</body>
```

![image-20220419200906219](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220419200906219.png)

#### 自定义列表

**场景：** 在网页的底部导航中通常会使用自定义列表实现

**标签组成：** 

|   标签名    |                  说明                   |
| :---------: | :-------------------------------------: |
| `<dl></dl>` | 表示自定义列表的整体，用于包裹dt/dd标签 |
| `<dt></dt>` |          表示自定义列表的主题           |
| `<dd></dd>` |  表示自定义列表的针对主题的每一项内容   |

> dl即definition list
>
> dt即difinition term
>
> dd即definition description

**显示特点：**

- `<dd>`前会默认显示缩进效果

**注意点：**

- `<dl>`标签只允许包含`<dt>`/`<dd>`标签
- `<dt>`/`<dd>`标签可以包含任意内容

**效果：**

```html
<body>
    <dl>
        <dt>帮助中心</dt>
        <dd>账户管理</dd>
        <dd>购物指南</dd>
    </dl>
</body>
```

![image-20220419201822893](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220419201822893.png)

### 表格标签

#### 基本标签

**场景：** 在网页中以行+列的单元格的方式整齐展示数据，如：学生成绩

**基本标签：**

|        标签名         |                             说明                             |
| :-------------------: | :----------------------------------------------------------: |
|   `<table></table>`   |               表示表格的整体，可用于包裹多个tr               |
|      `<tr></tr>`      |                    表示每行，可用于包裹td                    |
|      `<td></td>`      |                  表示单元格，可用于包裹内容                  |
| `<caption></caption>` |      表示表格整体大标题，默认在表格整体顶部居中位置显示      |
|      `<th></th>`      | 表示一列小标题，通常用于表格第一行，默认内部文字加粗并居中显示 |

> tr即table row
>
> td即table data cell
>
> th即table header cell

**注意点：**

- 嵌套关系：`<table>`->`<tr>`->`<td>`=`<th>`
- `<caption>`标签写在`<table>`标签内
- `<caption>`标签的内容不会加粗，需要加粗可以添加`<strong>`标签

**属性：** 写在`<table>`标签内的属性

| 属性名 | 属性值 |   效果   |
| :----: | :----: | :------: |
| border |  数字  | 边框宽度 |
| width  |  数字  | 表格宽度 |
| height |  数字  | 表格高度 |

实际开发中针对于样式效果推荐使用CSS设置

#### 结构标签

**场景：**因为表格可能很长，为了更好的表示表格的语义，可以将表格分割成：`表格头部` 和 `表格主体` 两大部分。

**标签：**

|      标签名       |   名称   |                             说明                             |
| :---------------: | :------: | :----------------------------------------------------------: |
| `<thead></thead>` | 表格头部 | 用于定义表格的头部，`<thead>` 内部必须拥有 `<tr>` 标签，且其中推荐放`<th>`标签 |
| `<tbody></tbody>` | 表格主体 |            用于定义表格的主体，主要用于放数据本体            |
| `<tfoot></tfoot>` | 表格底部 |                      用于定义表格的底部                      |

**注意点：**

- 表格结构标签内部用于包裹`<tr>`标签
- 表格结构标签可以省略
- 表格结构标签在浏览器中没有显示效果，帮助理解代码

#### 合并单元格

**场景：** 将水平或垂直多个单元格合并成一个单元格

**合并单元格三步曲：**

- 明确合并哪几个单元格
- 通过左上原则，确定保留谁删除谁
  - 跨行(上下合并)->只保留最上的，删除其他
  - 跨列(左右合并)->只保留最左的，删除其他


- 给保留的单元格设置：跨行合并或者跨列合并

**合并单元格的方式：**

- 跨行合并（上下合并）：`rowspan="合并单元格的个数"`
- 跨列合并（左右合并）：`colspan="合并单元格的个数"`

| 属性名  |      属性值      |               说明               |
| :-----: | :--------------: | :------------------------------: |
| rowspan | 合并单元格的个数 | 跨行合并，将多行的单元格垂直合并 |
| colspan | 合并单元格的个数 | 跨列合并，将多列的单元格水平合并 |

**注意：**

- 只有同一个结构标签中的单元格才能合并，不能跨结构标签合并(不能跨thead、tbody、tfoot)

#### 综合案例

**代码：**

```html
<body>
    <table border="1" width="600" height="100">
        <caption><strong>大标题</strong></caption>
        <!-- 表格头部 -->
        <thead>
            <tr>
                <th>标题1</th>
                <th>标题2</th>
                <th>标题3</th>
            </tr>
        </thead>

        <!-- 表格主体 -->
        <tbody>
            <tr>
                <td>1行, 1列</td>
                <td>1行, 2列</td>
                <td>1行, 3列</td>
            </tr>
            <tr>
                <td>2行, 1列</td>
                <td>2行, 2列</td>
                <td>2行, 3列</td>
            </tr>

            <!-- 合并单元格 -->
                <!-- 跨行合并 -->
            <tr>
                <td rowspan="2">3行1列和4行1列跨行合并，所以下边的单元格要删掉</td>
                <td>3行, 2列</td>
                <td>3行, 3列</td>
            </tr>
            <tr>
                <td>4行, 2列</td>
                <td>4行, 3列</td>
            </tr>
                <!-- 跨列合并 -->
            <tr>
                <td colspan="2">5行1列和5行2列跨列合并，所以右边的单元格要删掉</td>
                <td>5行, 3列</td>
            </tr>
                <!-- 又跨列又跨行 -->
            <tr>
                <td rowspan="2" colspan="2">5、6行的1、2列合并，只保留左上角的5行1列，其余都删除</td>
                <td>6行, 3列</td>
            </tr>    
            <tr>
                <td>7行, 3列</td>
            </tr>
        </tbody>

        <!-- 表格尾部 -->
        <tfoot>
            <tr>
                <th>总结</th>
                <td>总结内容1</td>
                <td>总结内容2</td>
            </tr>
        </tfoot>
    </table>
</body>
```

**效果：**

![image-20220419211639679](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220419211639679.png)

### 表单

> 表单： html页面中用来收集用户信息的所有元素集合，然后把这些信息发送给服务端

#### 表单组成

在 HTML 中，一个完整的表单通常由 `表单域`、`表单控件`（也称为表单元素）和 `提示信息` 3 个部分构成。

#### 表单域

**表单域是一个包含表单元素的区域。**

在 HTML 标签中，`<form>` 标签用于定义表单域，以实现用户信息的收集和传递。

`<form>` 会把它范围内的表单元素信息提交给服务器（表单元素控件需要添加`name`属性才能将收集的信息发送给服务器）。

```html
<form action="url地址" method="提交方式" name="表单域名称">
    <!-- 各种表单元素控件 -->
</form>
```

**常用属性：**

|  属性名  |     属性值     |                        作用                        |
| :------: | :------------: | :------------------------------------------------: |
| `action` |   `url` 地址   | 用于指定接收并处理表单数据的服务器程序的 url 地址  |
| `method` | `get` / `post` |  用于设置表单数据的提交方式，其取值为 get 或 post  |
|  `name`  |      名称      | 用于指定表单的名称，以区分同一个页面中的多个表单域 |

**form 表单中 method 的 get 和 post 区别：**

> method 方法规定如何发送表单数据（form-data）（表单数据会被发送到在 action 属性中规定的页面中）。
>
> 表单数据可被作为 URL 变量的形式来发送（method="get"）或者作为 HTTP post 事务的形式来发送（method="post"）。
>
> **关于 GET 的注释：**
>
> - 将表单数据以名称/值对的形式附加到 URL 中
> - URL 的长度是有限的（大约 3000 字符）
> - 绝不要使用 GET 来发送敏感数据！（在 URL 中是可见的，且浏览器会记录 URL）
> - 对于用户希望加入书签的表单提交很有用（因为信息记录在 URL 中，直接保存 URL 即可）
> - GET 更适用于非安全数据，比如在 Google 中查询字符串
>
> **关于 POST 的注释：**
>
> - 将表单数据附加到 HTTP 请求的 body 内（数据不显示在 URL 中）
> - 没有长度限制
> - 通过 POST 提交的表单不能加入书签

**表单控件必备的属性：**

- `name`属性：设置表单元素的名称，该名称是提交数据时的参数名
- `value`属性：设置表单元素的值，该值是提交数据时参数名所对应的值(单选框、复选框、下拉菜单用)

**表单提交时，数据成功发送给服务器的前提：**

1. 表单项(文本框、密码框等)中添加`name`属性值
2. 单选、复选、下拉列表中的`<option>`，需要添加`value`属性
3. 表单项需要写在提交的`<form>`标签中

#### 表单标签

##### input标签

**场景：** 在网页中显示收集用户信息的表单效果，如：登录页、注册页

**代码：**

```html
<input type="xxx"></input>
```

input标签可以通过**type属性值不同**，展示不同效果

**type属性值：**

| type属性值 |                   说明                   |
| :--------: | :--------------------------------------: |
|    text    |         文本框，用于输入单行文字         |
|  password  |           密码框，用于输入密码           |
|   radio    |            单选框，用于多选一            |
|  checkbox  |            复选框，用于多选多            |
|    file    |        文件选择，用于之后上传文件        |
|   submit   |            提交按钮，用于提交            |
|   reset    |            重置按钮，用于重置            |
|   button   | 普通按钮，默认无功能，之后配合js添加功能 |

**按钮更多单独使用button标签**

###### 文本框和密码框

**场景：** 在表单中中显示输入文字/密码的表单控件

**type属性值：**

- `text`：文本框

  ```html
  文本框：<input type="text" placeholder="请输入用户名">
  ```

- `password`：密码框

  ```html
  密码框：<input type="password" placeholder="请输入密码">
  ```

**常用属性：**

|   属性名    |             说明             |
| :---------: | :--------------------------: |
| placeholder | 占位符，即文本框中的提示文字 |

###### 单选框和复选框

**场景：** 在网页中显示多选一的单选表单控件

**type属性值：**

- `radio`：单选框

  ```html
  <input type="radio" name="gender">男
  <input type="radio" name="gender">女
  <input type="radio" name="gender" checked>外星人
  ```

- `checkbox`：复选框

  ```html
  <input type="checkbox" name="sport">游泳
  <input type="checkbox" name="sport">跑步
  <input type="checkbox" name="sport">骑车
  <input type="checkbox" name="sport" checked>躺平
  <input type="checkbox" name="sport" checked>睡觉
  ```

**常用属性：**

| 属性名  |                             说明                             |
| :-----: | :----------------------------------------------------------: |
|  name   | 分组，有相同name属性值的单选框为一组，一组中同时只能有一个被选中 |
| checked |                           默认选中                           |

###### 文件上传

**场景：** 在网页中显示文件选择的表单控件

**type属性值：** `file`

```html
<input type="file" multiple> 
```

**常用属性：**

|  属性名  |                说明                 |
| :------: | :---------------------------------: |
| multiple |             多文件选择              |
|  accept  | 限制可接收的文件类型: 例如JPEG或PNG |

**accept属性值示例：**

- `accept="image/png"`或者`accept=".png"`：接受 PNG 文件
- `accept="image/png, image/jpeg"`或者`accept=".png, .jpg, .jpeg"`： 接受 PNG 或 JPEG 文件

**注意点：**

- 文件上传需要单独一个表单，并添加使用post方法，并添加` enctype="multipart/form-data"`

```html
<form action="https://localhost:8080" method="post" enctype="multipart/form-data">
	<input type="file" name="filename" multiple>
	<input type="submit">
</form>
```

- 该标签提交的`value`属性包含所选文件的路径，如果选择多个文件，则该字符串表示第一个选定的文件；如果尚未选择文件，则该字符串为`""`（空）

###### 按钮

**场景：** 在网页中显示不同功能的按钮表单控件

**type属性值：**

- `reset`：重置按钮

```html
<input type="reset" value="重置按钮">
```

- `submit`：提交按钮

```html
<input type="submit" value="提交按钮">
```

- `button`：普通按钮

```html
<input type="button" value="普通按钮">
```

**注意点：**

- 普通按钮默认无功能，需使用js封装功能

- 使用`value`属性在按钮中显示提示文字
- 需要实现以上按钮功能，需配合form标签使用
- form标签使用方法：用`<form></form>`标签需要提交的表单数据和按钮一起包裹起来即可：

```html
<form>
    <!-- 文本框中输入的文字会在按下按钮后提交 -->
    <input type="text" placeholder="请输入用户名">
    <input type="submit" value="提交按钮">
</form>
```

##### button标签

**场景：** 在网页中显示用户点击的按钮

**标签名：** button

**type属性值：** （与input按钮系列相同）

| 属性名 |                   功能                   |
| :----: | :--------------------------------------: |
| submit |            提交按钮，用于提交            |
| reset  |            重置按钮，用于重置            |
| button | 普通按钮，默认无功能，之后配合js添加功能 |

```html
<button type="reset">重置按钮</button>
<button type="submit">提交按钮</button>
<button type="button">普通按钮</button>
```

**注意点：**

- 谷歌浏览器中button默认是提交按钮
- button标签是双标签，更便于包裹其他内容：文字、图片等（这些内容显示在按钮上）

##### select标签

**场景：** 在网页中提供多个选择项的下拉菜单表单控件

**标签组成：**

- `<select>`标签：下拉菜单的整体
- `<option>`标签：下拉菜单的每一项

```html
 城市：
 <select>
	<option>北京</option>
	<option>山西</option>
	<option selected>浙江</option>
</select>
```

**常见属性：**

- `selected`：下拉菜单的默认选中

##### textarea标签

**场景：** 在网页中提供可输入多行文本的表单控件

**标签名：** `textarea`

**常见属性：**

- `cols`：规定了文本域的可见宽度
- `rows`：规定了文本域的可见行数

```html
<textarea cols="30" rows="10"></textarea>
```

**注意点：**

- 右下角可以拖拽改变大小
- 实际开发时针对样式效果推荐使用CSS设置

##### label标签

**场景：** 常用于绑定内容与表单标签的关系

（单选/复选框中只能点击圆圈才能选中，绑定后可以点击内容就能选中）

**标签名：** `label`

**使用方法：**

- 方法一：
  1. 使用label标签把内容（如：文本）包裹起来
  2. 在表单标签上添加id属性
  3. 在label标签的for属性中设置对应的id属性值

```html
<input type="radio" name="gender" id="id_1">
<label for="id_1">男生</label>

<input type="radio" name="gender" id="id_2">
<label for="id_2">女生</label>
```

- 方法二：
  1. 直接使用label标签把内容和表单标签一起包裹起来
  2. 需要把label标签的for属性删除即可

```html
<label>
	<input type="radio" name="gender">男生
</label>
<label>
	<input type="radio" name="gender">女生
</label>
```

**效果：** 此时用户点击男生/女生就可以选中选项了

![image-20220420202650677](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220420202650677.png)

#### 表单实例

**使用表格标签对表单进行格式化，对齐表单提高显示效果**

**代码：**

```html
<form action="http://127.0.0.1:5500" method="get" name="测试表单">
    <table>
        <tr>
            <td>用户名称:</td>
            <td><input type="text" placeholder="请输入用户名" name="username"></td>
        </tr>
        <tr>
            <td>用户密码:</td>
            <td><input type="password" placeholder="请输入密码" name="password"></td>
        </tr>
        <tr>
            <td>确认密码:</td>
            <td><input type="password" placeholder="请输入密码" name="insur_password"></td>
        </tr>
        <tr>
            <td>性别:</td>
            <td>
                <label>
                    <input type="radio" name="gender" id="id_1" value="boy">男生
                </label>
                <label>
                    <input type="radio" name="gender" id="id_2" value="girl">女生
                </label>
            </td>
        </tr>
        <tr>
            <td>兴趣爱好:</td>
            <td>
                <label>
                    <input type="checkbox" name="interests" value="swim">游泳
                </label>
                <label>
                    <input type="checkbox" name="interests" value="foorball">足球
                </label>
                <label>
                    <input type="checkbox" name="interests" value="game">游戏
                </label>
            </td>
        </tr>
        <tr>
            <td>国籍:</td>
            <td>
                <select name="country">
                    <option selected value="china">中国</option>
                    <option value="america">美国</option>
                    <option value="japanese">日本</option>
                    <option value="canada">加拿大</option>
                </select>
            </td>
        </tr>
        <tr>
            <td>个人简介:</td>
        </tr>
        <tr>
            <td colspan="2">
                <textarea rows="10" cols="50" name="introduction">这个用户很懒，什么都没写</textarea>
            </td>
        </tr>
        <tr>
            <td><input type="reset" value="重置"></td>
            <td><input type="submit" value="提交"></td>
        </tr>
    </table>
</form>
```

**效果：**

![image-20220421230033382](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220421230033382.png)

**提交后的URL组成：**

```html
http://127.0.0.1:5500/
?username=abc
&password=123
&insur_password=123
&gender=boy
&interests=swim
&interests=foorball
&country=china
&introduction=%E5%BF%BD%E6%9C%89%E7%8B%82%E5%BE%92%E5%A4%9C%E7%A3%A8%E5%88%80%EF%BC%81
```

### 语义化标签

#### 无语义标签

**场景：** 实际开发网页时会大量频繁的使用`div`和`span`这两个没语义的布局标签

**`div`标签：** 一行只显示一个（div包裹的内容独占一行，无论前后是否有内容）

**`span`标签：** 一行可以显示多个

#### 有语义标签

**场景：** 在HTML5版本中，推出了一些有语义的布局标签供开发者使用（手机端用）

**标签：**

| 标签名  |    语义    |
| :-----: | :--------: |
| header  |  网页头部  |
|   nav   |  网页导航  |
| footer  |  网页底部  |
|  aside  | 网页侧边栏 |
| section |  网页区块  |
| article |  网页文章  |

![image-20220420204401479](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220420204401479.png)

**注意点：**

- 以上标签显示特点和div一致，但是比div多了不同的语义

## 内联框架

> `<iframe>`标签：用于在网页内内嵌一个网页，即网页上开辟一个小区域(子窗口)，显示一个单独网页

**语法：**

```html
<!-- URL指向隔离页面的位置 -->
<iframe src="URL"></iframe>
```

**iframe标签和a标签组合使用的步骤（即点击链接后由内嵌窗口跳转）：**

1. 在iframe标签中使用name定义一个名称
2. 在a标签的target属性上设置iframe的name属性值

**效果：**

```html
<input type="radio" name="gender" id="id_1">男
<input type="radio" name="gender" id="id_2">女
<br>
<a href="css.html" target="abc">点击这里，子窗口会跳转</a>
<br>
<iframe src="表格测试.html" width="610" height="300" name="abc"></iframe>
```

- 点击前：

  ![image-20220421215006174](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220421215006174.png)

- 点击后：

![image-20220421215028917](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220421215028917.png)

## 字符实体

### 空格合并现象

**场景：** 如果html代码中同时并列出现多个空格、换行、缩进等，最终浏览器只会解析出一个空格

### 常见的字符实体

| 显示结果 |     描述     |      实体名称       |
| :------: | :----------: | :-----------------: |
|   ` `    | 空格(最常用) |      `&nbsp;`       |
|   `<`    |    小于号    |       `&lt;`        |
|   `>`    |    大于号    |       `&gt;`        |
|   `&`    |     和号     |       `&amp;`       |
|   `"`    |     引号     |      `&quot;`       |
|   `'`    |     撇号     | `&apos;` (IE不支持) |
|   `￠`   |  分（cent）  |      `&cent;`       |
|   `£`    | 镑（pound）  |      `&pound;`      |

| 显示结果 |       描述        |  实体名称  |
| :------: | :---------------: | :--------: |
|   `¥`    |     元（yen）     |  `&yen;`   |
|   `€`    |   欧元（euro）    |  `&euro;`  |
|   `§`    |       小节        |  `&sect;`  |
|   `©`    | 版权（copyright） |  `&copy;`  |
|   `®`    |     注册商标      |  `&reg;`   |
|   `™`    |       商标        | `&trade;`  |
|   `×`    |       乘号        | `&times;`  |
|   `÷`    |       除号        | `&divide;` |

## 语言种类

```html
<html lang="zh-CN"> 
</html>
```

用来定义当前网页显示的主语言，书写在 `<html>` 标签内

- `en` 定义语言为英语
- `zh` 定义语言为中文

简单来说：定义为 `en` 就是面向英文用户的网页，定义为 `zh` 就是面向中国大陆用户的网页

> `en-GB` 英文（英国）
>
> `en-US` 英文（美国）
>
> `zh-CN` 中文（简体，中国大陆）
>
> `zh-SG` 中文（简体，新加坡）
>
> `zh-HK` 中文（繁体，香港）
>
> `zh-MO` 中文（繁体，澳门）
>
> `zh-TW` 中文（繁体，台湾）

语言的设置是为了方便 `浏览器搜索推荐` 以及触发 `浏览器翻译功能`，并不是说设置了某类主语言后网页中就不能存在其他类型的语言了



# 【CSS】

> **CSS：层叠样式表单(Cascading style sheets)** ，是用于(增强)控制网页样式并允许将样本信息与网页内容分离的一种标记性语言
>
> [CSS 参考手册 (w3school.com.cn)](https://www.w3school.com.cn/cssref/index.asp)

## 语法规则

```css
<head>
    <style>
		/**
		  *	p:选择器(这里选择的是段落)
		  * color:属性名
		  * red:属性值
		  * 一般一行值描述一个属性
		  **/
        p {
            color: red;
        }
    </style>
</head>
```

**选择器：** 浏览器根据选择器决定受CSS样式影响的HTML元素(标签)

**属性：** 要改变的样式名，每个属性都有一个值。属性和值被冒号进行分开，并且由花括号包围，组成一个完整的样式声明（declaration）

**多个声明：** 若定义不止一个声明，则需要用分号将每个声明分开。（最后一条声明可以不加分号，但尽量在每条声明的末尾都加上分号）



## 注释

```css
/* CSS使用斜杠注释 */
```

## 引入方式

**CSS有三种引入方式（即和HTML结合的方式）：**

- **内嵌式：** CSS写在标签的`<style>`标签中

  ​	提示：`<style>`标签虽然可以写在页面任意位置，但是通常约定写在`head`标签中

  - 缺点：只能在同一页面中复用代码，维护起来也不方便

```html
<head>
    <!-- head其他部分省略 -->
    <style type="text/css">
        /* 这里要用css注释，不能用html注释 */
        div {
            border: 1px solid red;
        }
    </style>
</head>
<body>
    <div>div 标签1</div>
    <div>div 标签2</div>
</body>
```

如上这样写，该样式对`<body>`中的所有`<div>`标签均生效

- **行内式：** CSS写在标签的style属性中
  - 缺点：样式多了，复杂，可读性差

```html
<div style="border: 1px solid red" >div 标签1</div>
```

- **外联式：** CSS写在一个单独的.css文件中，再通过link标签引入

  这样做，只需要修改css文件就能对所有html样式进行修改

​	css文件：

```css
div{
    border: 1px solid red;
}
```

​	html文件：

```html
<head>
    <!-- head其他部分省略 -->
    <link rel="stylesheet" type="text/css" href="css样式.css">
</head>
```

## 选择器

- **标签名选择器：** 决定哪些标签被动地使用这些样式

```css
标签名{
    属性名: 属性值;
}
/* 该样式对div标签生效 */
div{
    border: 1px solid red;
}

/* html文档里的div标签会使用该样式 */
<div>标签1</div>
```

- **ID选择器：** 通过id属性选择性地使用样式

```css
#id{
    属性名: 属性值;
}

/* 只有id为001的div标签会生效 */
#id001 {
	color: blue;
	font-size: 30px;
	border: 1px yellow solid
}

/* html文档里定义id属性 */
<div id="id001">标签1</div>
<div id="id002">标签2</div>
```

- **class选择器：** 类型选择器，通过class属性选择性地分组使用样式

```css
.class{
    属性名: 属性值
}

/* 只有class为class001的div标签都会生效 */
#class001 {
	color: blue;
	font-size: 30px;
	border: 1px yellow solid
}

/* html文档里定义class属性 */
<div class="class001">标签1</div>
<div class="class001">标签2</div>
<div class="class003">标签3</div>
```

- **组合选择器：** 组合选择器可以让多个选择器共用同一样式

```css
选择器1, 选择器2, 选择器n{
	属性名: 属性值;
}

/* 三个选择器都会生效 */
#id001, #class001, span{
	color: blue;
	font-size: 30px;
	border: 1px yellow solid
}

/* html文档 */
<div class="class001">标签1</div>
<div class="class001">标签2</div>
<div id="id001">标签3</div>
<span>标签4</span>
```

## 常用样式

|     样式名     |                   代码                    |                     说明                     |
| :------------: | :---------------------------------------: | :------------------------------------------: |
|    字体颜色    |                  `color`                  |                 修改字体颜色                 |
|    字体大小    |                `font-size`                |             修改字体大小(单位px)             |
|      边框      |                 `border`                  |                   修改边框                   |
|      宽度      |                  `width`                  |        修改宽度(要先设置边框，单位px)        |
|      高度      |                 `height`                  |        修改高度(要先设置边框，单位px)        |
|    背景颜色    |            `background-color`             |                 修改背景颜色                 |
|    div居中     | `margin-left: auto`和`margin-right: auto` | 设置相对页面居中(是标签所渲染的块，不是文字) |
|    文本居中    |           `text-align: center`            |  设置文字居中(修改属性值可以左对齐和右对齐)  |
| 超链接去下划线 |          `text-decoration:none`           |              去掉超链接的下划线              |

|   样式名   |            代码             |            说明            |
| :--------: | :-------------------------: | :------------------------: |
|  表格细线  | `border-collapse: collapse` | 将表格单元格线和边框线合并 |
| 无符号列表 |     `list-style: none`      | 去掉列表的修饰符(点、序号) |



# 【JavaScript】

> JavaScript 是属于 HTML 和 Web 的编程语言。运行在客户端上，需要浏览器来解析执行JavaScript代码。
>
> JavaScript 是 web 开发者必学的三种语言之一：
>
> - *HTML* 定义网页的内容
> - *CSS* 规定网页的布局
> - *JavaScript* 对网页行为进行编程
>
> JavaScript 和 Java 不能说关系密切，只能说是毫无关系，不论是概念还是设计。
>
> - JS是弱类型，定义变量后类型可变
> - Java是强类型，定义变量后类型确定，不可变
>
> JS特点：
>
> - 交互性（它可以做的就是信息的动态交互）
> - 安全性（不允许直接访问本地硬盘）
> - 跨平台性（只要是可以解释JS的浏览器都可以执行，和平台无关）
>
> [JavaScript 帮助文档_w3cschool](https://www.w3cschool.cn/wkjavascript/xzn61o9l.html)

## 注释

```js
/* js使用斜杠注释 */
```

## 引入方式

- **内部引入：** 在`<head>`标签或`<body>`标签中，使用`script`标签来书写`JavaScript`代码

```html
<head>
    <!-- head其他部分省略 -->
    <script>
        /**
         * alert是JavaScript提供的一个警告框函数
         * 它可以接收任意类型的参数，这个参数就是警告框的提示信息
         */
        alert();
    </script>
</head>

<body>
    <script type="text/javascript">
        alert();
    </script>
</body>
```

- **外部引入：** 将js代码写在单独的.js文件里，html文档中通过`<script>`标签的`src`属性引入

```html
 <!-- 使用script的src属性引入外部的js文件来执行 -->
<script type="text/javascript" src="jstext.js"></script>
```

**注意：** 两种功能在同一个`<script>`标签中不能同时使用，即`<script>`标签用于引入外部js文件后标签内不能再书写js代码

## 变量

**JavaScript的变量类型：** js是弱类型语言，对数据类型要求不严格

- **值类型(基本类型)**：基本类型值在内存中占据固定大小，直接存储在栈内存中的数据

|  类型  |   说明    |
| :----: | :-------: |
|  数字  |  Number   |
| 字符串 |  String   |
|  布尔  |  Boolean  |
|  对空  |   Null    |
| 未定义 | Undefined |

- **引用数据类型（对象类型）：**引用类型在栈中存储了指针，这个指针指向堆内存中的地址，真实的数据存放在堆内存里。

| 类型 |   说明   |
| :--: | :------: |
| 对象 |  Object  |
| 数组 |  Array   |
| 函数 | Function |

**JavaScript里特殊的值：**

|     值      |                    说明                     |
| :---------: | :-----------------------------------------: |
| `undefined` | 未定义，所有js变量未赋初始值时都是undefined |
|   `null`    |                    空值                     |
|    `NAN`    |     全称：Not a Number。非数字，非数字      |

**JavaScript定义变量：**

```js
var 变量名;
var 变量名 = 值;

var i; // undefined
var j = 12;
// typeof()可以去变量的类型返回
alert(typeof(i)) // 结果为number
i = "abc" // js的字符串可是用单引号，也可以用双引号，但要遵循成对嵌套使用的原则
alert(typeof(i)) // 结果为String
alert(typeof(i * j)) // 结果为NAN
```

## 关系运算

**JS关系运算符大致和Java一样**

**特殊的运算符: **

| 运算符 |  中文  |                        说明                        |
| :----: | :----: | :------------------------------------------------: |
|   ==   |  等于  |                 简单地做字面值比较                 |
|  ===   | 全等于 | 除了做字面值的比较之外，还会比较两个变量的数据类型 |

```js
"12" == 12 // true
"12" === 12 // false
```

## 逻辑运算

**JS逻辑运算符和Java一样，但有不少区别：**

- 在JavaScript语言中，所有的变量都可以作为一个boolean类型的变量去使用，也就是说都是"有值"的变量都是真
- `0`、`null`、`undefined`、`""`(空串)默认值为`false`

- | 运算符 |   中文   |                             说明                             |
  | :----: | :------: | :----------------------------------------------------------: |
  |  `&&`  |  且运算  | 当表达式全为真的时候，返回最后一个表达式的值；当表达式中有一个值为假的时候，返回第一个为假的表达式的值 |
  |  `||`  |  或运算  | 当表达式全为假的时候，返回最后一个表达式的值；当表达式中有一个值为真的时候，返回第一个为真的表达式的值 |
  |  `!`   | 取反运算 |                        真为假，假为真                        |

- `&&`和`||` 有短路，也就是说当运算有
- 结果后，后面的表达式不再执行

## 数组

**数组定义方式：**

```js
// 静态声明数组
var array = ['a', 'b', 'c'];
// 动态声明数组，元素个数为0
var array1 = new Array();
// 动态声明数组，元素个数为5
var array2 = new Array(5)

//空数组
var 数组名 = []; 
// 数组中元素数据类型可以不一样
var 数组名 = [1, "abc", ture] 

// JS数组只要通过数组下标赋值，那么最大下标值会自动给数组做扩容操作(读取时不行，即alert(array1[9]不行，读取结果为undefined)
array1[0] = 12; // 数组长度自动+1，从0变成1
array1[2] = "abc"; // 数组长度变为3
alert(array1[1]); // undefined
```

**数组遍历：**

```js
for(var i = 0; i < arr.length; i++) {
    alert(arr[i]);
}
```

## 函数

### 定义方式

- **第一种：使用function关键字定义函数**

```js
// 无参函数
function fun1() {
    alert("无参函数被调用了");
}

// 有参函数，js形参不需要声明变量类型
function fun2(a, b) {
    alert("有参函数被调用了，a=>" + a + ", b=>" + b);
}

// 带返回值的函数，直接返回即可
function fun3(a, b) {
    var result = a + b;
    return result;
}
```

- **第二种：** 函数表达式(匿名函数)

```js
// 通过表达式定义函数x
var x = function (a, b) {
    return a * b
};

// 调用函数x，获得返回值
var z = x(3, 4);
```

**JS中的函数不允许重载，每次定义都会覆盖上一次的定义**

### 隐形参数

**`arguments`：在function函数中不需要定义，但却可以直接用来获取所有参数的变量(是一个数组)，类似Java中的可变长参数**

```js
function fun4(a, b) {
    // 使用隐形参数获取参数a和b
    var result = arguments[0] + arguments[1];
    return result;
}

// arguments在无参函数中也可以接收到参数
function fun5() {
	alert(arguments.length);
}
fun5(1,2,3);
```

## 自定义对象

> "JavaScript 对象是属性和方法的容器"

### 定义对象

- **第一种定义方法：**

```js
// 定义对象
var 变量名 = new Object[];    	// 对象实例
变量名.属性名 = 值;             // 定义一个属性
变量名.函数名 = function() {};  // 定义一个函数

// 例子
var obj = new Object[];
obj.name = "小明";
obj.fun = function(a ,b) {
    alert("姓名:" + this.name);
    return a + b;
};
```

- **第二种定义方法：**

```js
var 变量名 = {
    属性名: 值, // 定义一个属性
    属性名: 值,	// 属性、函数之间用逗号隔开
    函数名: function() {} // 定义一个函数，对象的最后一个属性/函数尾不加逗号
}; // 记得加分号

// 例子
var person = {
    name:"John",
    age:50,
    getAge: function() {
        alert("年龄：" + this.age)
    }
};
```

### 访问对象

```js
var person = {
    name:"John",
    age:50,
    getAge: function() {
        alert("年龄：" + this.age);
    }
};

// 访问对象属性的两种方法
person.name;	// 方法一
person["name"];	// 方法二

// 访问对象方法
var name = person.getAge();
```



## DOM对象

#### 简介

**DOM，全称“Document Object Model（文档对象模型）”，定义了访问和操作HTML文档的标准方法**

DOM把整个页面规划成由结点层级构成的树状模型，用户通过相应的方法可以访问页面中的任何一个结点。

**人话： 把文档中的标签、属性、文本转换成一个对象来管理**

DOM是这样规定的：

- 整个HTML文档是一个文档结点
- 每个HTML标签是一个元素结点
- 包含在HTML元素中的文本是文本结点
- 每一个HTML属性是一个属性结点
- 注释属于注释结点

HTML文档中的所有结点组成了一个文档树：

![image-20220422191417134](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220422191417134.png)

**通过DOM对象获得标签对象：**

```html
// js中通过dom获取标签对象
var a = document.getElementById("id01");

<!-- html标签中要使用id属性 -->
<button id="id01"></button>
```

#### 查找元素

| 方法                                    | 描述                   |
| :-------------------------------------- | :--------------------- |
| `document.getElementById(id)`           | 通过元素 id 来查找元素 |
| `document.getElementsByTagName(name)`   | 通过标签名来查找元素   |
| `document.getElementsByClassName(name)` | 通过类名来查找元素     |

**注意点：**

- 优先顺序: `id`>`name`>`className`
- 只有在页面加载完成之后调用这几个方法才有效

#### 改变元素

| 方法                                     | 描述                   |
| :--------------------------------------- | :--------------------- |
| `element.innerHTML = new html content`   | 改变元素的 inner HTML  |
| `element.attribute = new value`          | 改变 HTML 元素的属性值 |
| `element.setAttribute(attribute, value)` | 改变 HTML 元素的属性值 |
| `element.style.property = new style`     | 改变 HTML 元素的样式   |

**innerHTML即标签之间的文本，比如`<div>innerHTML</div>`**

#### 添加和删除元素

| 方法                              | 描述             |
| :-------------------------------- | :--------------- |
| `document.createElement(element)` | 创建 HTML 元素   |
| `document.removeChild(element)`   | 删除 HTML 元素   |
| `document.appendChild(element)`   | 添加 HTML 元素   |
| `document.replaceChild(element)`  | 替换 HTML 元素   |
| `document.write(text)`            | 写入 HTML 输出流 |

```js
var divObj = document.createElement("div");
divObj.innerHtml = "hello world" // <div>hello world</div> 这个标签还在内存中
// 添加子元素
document.body.appendChild(divObj);
```

**注意文本也是一个结点，`"hello world"`是`<div>`标签的子节点，所以第二行代码也可以写成`divObj.appendChild("hello world")`**

#### 添加事件处理程序

| 方法                                                     | 描述                            |
| :------------------------------------------------------- | :------------------------------ |
| `document.getElementById(id).onclick = function(){code}` | 向 onclick 事件添加事件处理程序 |

### 实例

> **验证内容合法性**
>
> 需求：当用户点击了校验按钮，要获取输出框的内容，然后验证其是否合法
>
> 验证的规则是，必须由字母，数组组成

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        function onclickFun() {
            // 1、当我们要操作一个标签的时候，一定要先获取这个标签的对象
            var usernameObj = document.getElementById("username");
            // 2、获取对象中的内容
            var usernameText = usernameObj.value;
            // 3、正则表达式验证字符串
            var patt = /^\w{5,12}$/;
            // test用于测试某个字符串是否匹配规则
            if (patt.test(usernameText)) {
                return true;
            } else{
                alert("不合法");
                return false;
            }
        }
    </script>
</head>

<body>
    <form action="https://localhost:8080" method="get">
        用户名：<input type="text" id="username">
        <input type="button" value="校验" onclick="onclickFun();">
    </form>
</body>
```

### 帮助文档

> [JavaScript HTML DOM 文档 (w3school.com.cn)](https://www.w3school.com.cn/js/js_htmldom_document.asp)

## 事件

> 事件是电脑输入设备与页面进行交互的响应
>
> HTML 事件可以是浏览器行为，也可以是用户行为。
>
> 在HTML中使用JavaScript，在事件触发时JS可以执行一些代码。

### 常用事件

|   事件名   |       说明       |                   常见用途                   |
| :--------: | :--------------: | :------------------------------------------: |
|  `onload`  |   加载完成事件   | 页面加载完成后，常用于做页面js代码初始化操作 |
| `onclick`  |     点击事件     |           常用于按钮的点击响应操作           |
|  `onblur`  |   失去焦点事件   | 常用于输入框失去焦点后验证其输入内容是否合法 |
| `onchange` | 内容发生改变事件 |   常用于下拉列表和输入框内容发生改变后操作   |
| `onsubmit` |   表单提交事件   |   常用于表单提交前，验证所有表单项是否合法   |

### 事件注册

> 事件注册（绑定）：告诉浏览器，当事件响应后要执行哪些操作代码

**事件注册分为静态注册和动态注册两种：**

- 静态注册：通过html标签的事件属性直接赋予事件响应后的代码
- 动态注册：通过js代码得到标签的dom对象，再通过`dom对象.事件名 = function() {}`的形式

### onload事件

> **加载完成事件**

```html
<!-- 静态注册 -->
<body onload="alert();"></body>

<!-- 动态注册 -->
<script type="text/javascript">
        // onload事件动态注册,步骤固定
        window.onload = alert();       
</script> 
```

**HTML文档的代码执行顺序是自上而下，而onload事件在文档加载完成后加载，为了能够保证操作的模块或对象在js代码之前就加载，可以把js代码写在onload事件的处理函数里**

### onclick事件

> **点击事件**

```html
<!-- 静态注册 -->
<button onclick="alert();">按钮1</button>
 
<!-- 动态注册 --> 
<script type="text/javascript">  
        window.onload = function() {
            // 1.通过dom对象获取标签对象
            var btnObj = document.getElementById("001");
            // 2.标签对象.事件名 = function(){}
            btnObj.onclick = alert();
        };
</script>
```

### onblur事件

> 失去焦点事件

```html
<!-- 静态注册 -->
<!-- console是控制台变量，由js提供，专门用来向浏览器的控制器打印输出，用于测试使用 -->
用户名:<input type="text" onblur="console.log('静态注册onblur事件');">

<!-- 动态注册 -->
<script type="text/javascript">
        window.onload = function () {
            // 1.获取标签对象
            var onblurFun = document.getElementById("001");
            // 2.标签对象.事件名 = function(){}
            onblurFun.onblur = console("动态注册onblur事件");
        };
</script>       
```

### onchange事件

> 内容发生改变事件

```html
<!-- 静态注册 -->
<select onchange="console.log('静态注册onchange事件');">
    <option>中国</option>
    <option>美国</option>
    <option>加拿大</option>
</select>

<!-- 动态注册 -->
<script type="text/javascript">
    window.onload = function () {
        // 1.获取标签对象
        var onchangeFun = document.getElementById("001");
        // 2.标签对象.事件名 = function(){}
        onchangeFun.onchange = console.log("动态注册onchange事件");
    };
</script>
```

### onsubmit事件

> 表单提交事件

```html
<!-- 静态注册: 使用return false阻止表单提交，以过滤不合法信息 -->
<form action="https://localhost:8080" method="get" onsubmit="return false;">
    <input type="submit" value="静态注册">
</form>

<!-- 动态注册 -->
<script type="text/javascript">
    window.onload = function () {
        // 1.获取标签对象
        var onsubmitFun = document.getElementById("001");
        // 2.标签对象.事件名 = function(){}
        onsubmitFun.onsubmit = function() {
            return false;
        };
    };
</script>
```

## 常用正则表达

> [正则表达式 – 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/regexp/regexp-tutorial.html)
>
> [JavaScript 正则表达式 | 菜鸟教程 (runoob.com)](https://www.runoob.com/js/js-regexp.html)

- 邮箱号：允许-和_

```java
"^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(.[a-zA-Z0-9_-]+)+$"
```

- 密码：必须是6-20位的字母、数字、英文句号或下划线

```java
"^[.\\w_]{6,20}$"
```

# 【JSP】

## 简介

> JSP（全称：Java Server Pages）：Java 服务端页面。
>
> JSP是一种动态的网页技术，其中既可以定义 HTML、JS、CSS等静态内容，还可以定义 Java代码的动态内容，也就是 `JSP = HTML + Java`。
>
> 如下就是jsp代码：
>
> ```java
> <html>
>     <head>
>         <title>Title</title>
>     </head>
>     <body>
>         <h1>JSP,Hello World</h1>
>         <%
>         	System.out.println("hello,jsp~");
>         %>
>     </body>
> </html>
> ```

## 快速入门

- **导入JSP依赖**

  ```xml
  <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
  <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.3</version>
      <scope>provided</scope>
  </dependency>
  ```

  该依赖的`scope`必须设置为`provided`，因为tomcat中有这个jar包了，所以在打包时不希望将该依赖打进到工程的war包中。

- **创建JSP页面**

  在`webapp`目录下创建一个hello.jsp页面

- **编写代码**

  ```java
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <h1>hello jsp</h1>
      <%
          System.out.println("hello,jsp~");
      %>
  </body>
  </html>
  ```

- 启动tomcat并在浏览器中输入`http://localhost:8080/web-demo/hello.jsp`

  ![image-20220704114004620](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220704114004620.png)

## 原理

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20210818111039350.png)

**JSP本质上就是一个 Servlet**

1. 浏览器第一次访问`hello.jsp`页面
2. `tomcat`会将`hello.jsp`转换为名为`hello_jsp.java`的一个`Servlet`
3. `tomcat`再将转换的`servlet`编译成字节码文件`hello_jsp.class`
4. `tomcat` 会执行该字节码文件，向外提供服务

## 脚本

> JSP脚本用于在 JSP页面内定义 Java代码。



JSP 脚本有如下三个分类：

* `<%...%>`：内容会直接放到`_jspService()`方法之中
* `<%=...%>`：内容会放到`out.print()`中，作为`out.print()`的参数
* `<%!...%>`：内容会放到`_jspService()`方法之外，被类直接包含

**代码演示：**

- `<%...%>`

  在 `hello.jsp` 中书写

  ```jsp
  <%
      System.out.println("hello,jsp~");
      int i = 3;
  %>
  ```

  通过浏览器访问 `hello.jsp` 后，查看转换的 `hello_jsp.java` 文件，i 变量定义在了 `_jspService()` 方法中

​		<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20210818123606231.png" alt="image-20210818123606231" style="zoom:80%;" />

- `<%=...%>`

  在 `hello.jsp` 中书写

  ```jsp
  <%="hello"%>
  <%=i%>
  ```

  通过浏览器访问 `hello.jsp` 后，查看转换的 `hello_jsp.java` 文件，该脚本的内容被放在了 `out.print()` 中，作为参数

​		<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20210818123820571.png" alt="image-20210818123820571" style="zoom:80%;" />

- `<%!...%>`

  在 `hello.jsp` 中书写

  ```jsp
  <%!
      void  show(){}
  	String name = "zhangsan";
  %>
  ```

  通过浏览器访问 `hello.jsp` 后，查看转换的 `hello_jsp.java` 文件，该脚本的内容被放在了成员位置

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20210818123946272.png" alt="image-20210818123946272" style="zoom:80%;" />

  



## 缺点

由于 JSP页面内，既可以定义 HTML 标签，又可以定义 Java代码，造成了以下问题：

* **书写麻烦:** 特别是复杂的页面，既要写 HTML 标签，还要写 Java 代码

* **阅读麻烦:** 前端和后端混在一起

* **复杂度高:** 运行需要依赖于各种环境，JRE，JSP容器，JavaEE…

* **占内存和磁盘:** JSP会自动生成.java和.class文件占磁盘，运行的是.class文件占内存

* **调试困难:** 出错后，需要找到自动生成的.java文件进行调试

* **不利于团队协作:** 前端人员不会 Java，后端人员不精HTML

  如果页面布局发生变化，前端工程师对静态页面进行修改，然后再交给后端工程师，由后端工程师再将该页面改为 JSP 页面，很麻烦，不利于团队写作

由于上述的问题， JSP 已逐渐退出历史舞台，以后开发更多的是使用 HTML +  Ajax来替代。

## EL表达式

### 概述

> EL（全称Expression Language ）表达式语言，用于简化 JSP 页面内的 Java 代码。
>
> **EL 表达式的主要作用是获取数据。** 其实就是从域对象中获取数据，然后将数据展示在页面上。
>
> 而 EL 表达式的语法也比较简单，`${expression}` 
>
> 例如：${brands} 就是获取域中存储的key为brands的数据。

### 代码演示

- 定义servlet，在 servlet 中封装一些数据并存储到 request 域对象中并转发到 `el-demo.jsp` 页面。

```java
@WebServlet(name = "ServlerDemo1", value = "/demo1")
public class ServlerDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1. 准备数据
        List<Brand> brands = new ArrayList<Brand>();
        brands.add(new Brand(1,"三只松鼠","三只松鼠",100,"三只松鼠，好吃不上火",1));
        brands.add(new Brand(2,"优衣库","优衣库",200,"优衣库，服适人生",0));
        brands.add(new Brand(3,"小米","小米科技有限公司",1000,"为发烧而生",1));
        
        // 2.存储到request域中
        request.setAttribute("brands", brands);
        
        // 3.转发到el.demo.jsp
        request.getRequestDispatcher("/el-demo.jsp").forward(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

- 在 `el-demo.jsp` 中通过 EL表达式 获取数据

```html
<!-- isELIgnored="false" -->
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${brands}
</body>
</html>
```

- 浏览器显示效果：

```
[Brand{i=1, name='三只松鼠', s1='三只松鼠', j=100, s='三只松鼠，好吃不上火', k=1}, Brand{i=2, name='优衣库', s1='优衣库', j=200, s='优衣库，服适人生', k=0}, Brand{i=3, name='小米', s1='小米科技有限公司', j=1000, s='为发烧而生', k=1}]
```

### 域对象

JavaWeb中有四大域对象，分别是：

* page：当前页面有效
* request：当前请求有效
* session：当前会话有效
* application：当前应用有效

el 表达式获取数据，会依次从这4个域中寻找，直到找到为止。

而这四个域对象的作用范围如下图所示

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20210818152857407.png" alt="image-20210818152857407" style="zoom:60%;" />

例如： `${brands}`，会先从page域对象中获取数据，如果没有再到 requet 域对象中获取数据，如果再没有再到 session 域对象中获取，如果还没有才会到 application 中获取数据。

## JSTL标签

### 概述

JSP标准标签库(Jsp Standarded Tag Library) ，使用标签取代JSP页面上的Java代码。

例如：用`c:if`标签取代`if`语句

```jsp
<c:if test="${flag == 1}">
    男
</c:if>
<c:if test="${flag == 2}">
    女
</c:if>
```

![image-20220708091736179](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220708091736179.png)

### 安装

> **Apache Tomcat安装JSTL 库步骤如下：**
>
> 从Apache的标准标签库中下载的二进包(jakarta-taglibs-standard-current.zip)。
>
> - 官方下载地址：http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/
>
> 下载 **jakarta-taglibs-standard-1.1.2.zip** 包并解压，将 **jakarta-taglibs-standard-1.1.2/lib/** 下的两个 jar 文件：**standard.jar** 和 **jstl.jar** 文件拷贝到 **/WEB-INF/lib/** 下。
>
> 将 tld 下的需要引入的 tld 文件复制到 WEB-INF 目录下。
>
> **Maven依赖：**
>
> ```xml
> <dependency>
>     <groupId>jstl</groupId>
>     <artifactId>jstl</artifactId>
>     <version>1.2</version>
> </dependency>
> <dependency>
>     <groupId>taglibs</groupId>
>     <artifactId>standard</artifactId>
>     <version>1.1.2</version>
> </dependency>
> ```
>
> **JSTL标签库：**
>
> ```jsp
> <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
> ```

### if标签

`<c:if>`：相当于`if`判断

**属性：** test，用于定义条件表达式

```jsp
<c:if test="${flag == 1}">
    男
</c:if>
<c:if test="${flag == 2}">
    女
</c:if>
```

**代码演示：**

- 定义一个`servlet`，在该`servlet`中向request域对象中添加 键是`status`，值为`1`的数据：

```java
@WebServlet(name = "ServletDemo2", value = "/demo2")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 存储数据到request域中
        request.setAttribute("status",1);

        //2. 转发数据到 jstl-if.jsp
        request.getRequestDispatcher("/jstl-if.jsp").forward(request,response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

- 定义 `jstl-if.jsp` 页面，在该页面使用 `<c:if>` 标签

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <%-- c:if：来完成逻辑判断,替换java中的if else --%>
    <c:if test="${status == 1}">
        启用
    </c:if>

    <c:if test="${status  == 0}">
        禁用
    </c:if>
</body>
</html>
```

- 浏览器显示效果：

```
启用
```

### foreach标签

#### 用法一

`<c:forEach>`：类似于Java中的增强for循环

**属性：**

* items：被遍历的容器
* var：遍历产生的临时变量
* varStatus：遍历状态对象
  * count：从1开始计数
  * index：下标，从0开始计数

**代码演示：**

- Servlet：

```java
@WebServlet(name = "ServlerDemo1", value = "/demo1")
public class ServlerDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1. 准备数据
        List<Brand> brands = new ArrayList<Brand>();
        brands.add(new Brand(1,"三只松鼠","三只松鼠",100,"三只松鼠，好吃不上火",1));
        brands.add(new Brand(2,"优衣库","优衣库",200,"优衣库，服适人生",0));
        brands.add(new Brand(3,"小米","小米科技有限公司",1000,"为发烧而生",1));

        // 2.存储到request域中
        request.setAttribute("brands", brands);

        // 3.转发到el.demo.jsp
        request.getRequestDispatcher("/jstl-foreach.jsp").forward(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

- jsp：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<input type="button" value="新增"><br>
<hr>
<table border="1" cellspacing="0" width="800">
    <tr>
        <th>序号</th>
        <th>品牌名称</th>
        <th>企业名称</th>
        <th>排序</th>
        <th>品牌介绍</th>
        <th>状态</th>
        <th>操作</th>
    </tr>

    <c:forEach items="${brands}" var="brand" varStatus="status">
        <tr align="center">
                <%--<td>${brand.id}</td>--%>
            <td>${status.count}</td>
            <td>${brand.brandName}</td>
            <td>${brand.companyName}</td>
            <td>${brand.ordered}</td>
            <td>${brand.description}</td>
            <c:if test="${brand.status == 1}">
                <td>启用</td>
            </c:if>
            <c:if test="${brand.status != 1}">
                <td>禁用</td>
            </c:if>
            <td><a href="#">修改</a> <a href="#">删除</a></td>
        </tr>
    </c:forEach>
</table>
</body>
</html>
```

- 浏览器效果：

![image-20220708094356850](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220708094356850.png)

#### 用法二

`<c:forEach>`：类似于Java中的普通for循环

**属性：**

* begin：开始数
* end：结束数
* step：步长

**代码演示：** 从0循环到1，变量名为`i`，每次自增1

```jsp
<c:forEach begin="0" end="10" step="1" var="i">
    ${i}
</c:forEach>
```

# 【AJAX】

## 简介

> `AJAX` (Asynchronous JavaScript And XML)：异步的 JavaScript 和 XML
>
> AJAX的作用有以下两方面：
>
> 1. **与服务器进行数据交换:** 通过AJAX可以给服务器发送请求，服务器将数据直接响应回给浏览器（可以使用HTML+AJAX来替换JSP）
> 2. **异步交互:** 可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页，如：搜索搜索即时弹窗、用户名是否可用校验，等等

## 异步和同步

**同步发送请求：**

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210824001443897.png" alt="image-20210824001443897" style="zoom:80%;" />

浏览器页面在发送请求给服务器，在服务器处理请求的过程中，浏览器页面不能做其他的操作。只能等到服务器响应结束后才能，浏览器页面才能继续做其他的操作。

**异步发送请求：**

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210824001608916.png" alt="image-20210824001608916" style="zoom:80%;" />

浏览器页面发送请求给服务器，在服务器处理请求的过程中，浏览器页面还可以做其他的操作

## 快速入门

### 服务端实现

```java
@WebServlet("/ajaxServlet")
public class AjaxServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 响应数据
        response.getWriter().write("hello ajax~");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

### 客户端实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    //1. 创建核心对象
    var xhttp;
    if (window.XMLHttpRequest) {
        xhttp = new XMLHttpRequest();
    } else {
        // code for IE6, IE5
        xhttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
    //2. 发送请求
    xhttp.open("GET", "http://localhost:8080/ajaxServlet");
    xhttp.send();

    //3. 获取响应
    xhttp.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            alert(this.responseText);
        }
    };
</script>
</body>
</html>
```

## 案例

### 分析

**需求：** 在完成用户注册时，当用户输入框失去焦点时，校验用户名是否在数据库中已存在

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210824201415745.png" alt="image-20210824201415745" style="zoom:60%;" />

**前端逻辑：**

- 给用户名输入框绑定光标失去焦点事件
- 发送ajax请求，携带username参数
- 处理响应：是否显示信息

**后端逻辑：**

- 接收用户名
- 调用service层查询User
- 返回标记

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210829183854285.png" alt="image-20210829183854285" style="zoom:80%;" />

### 后端实现

```java
@WebServlet("/selectUserServlet")
public class SelectUserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.接收用户名
        String username = request.getParameter("username");
        // 2.调用service查询User对象，此处不进行业务逻辑处理，直接给flag赋值为 true，表明用户名占用
        boolean flag = true;
        // 3.响应标记
        response.getWriter().write("" + flag);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

### 前端实现

- **第一步：给用户名输入框绑定光标失去焦点事件`onblur`**

```js
// 1.给用户名输入框绑定 失去焦点事件
document.getElementById("username").onblur = function () {  
}
```

- **第二步：发送ajax请求，携带username参数**

```js
// 2. 发送ajax请求
// 获取用户名的值
var username = this.value;
// 2.1. 创建核心对象
var xhttp;
if (window.XMLHttpRequest) {
    xhttp = new XMLHttpRequest();
} else {
    // code for IE6, IE5
    xhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
// 2.2. 发送请求
xhttp.open("GET", "http://localhost:8080/selectUserServlet?username="+username);
xhttp.send();

// 2.3. 获取响应
xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
        //处理响应的结果
    }
};
```

> 由于发送的是GET请求，所以需要在URL后拼接从输入框获取的用户名数据。而第一步绑定的匿名函数中通过以下代码可以获取用户名数据：
>
> ```js
> // 获取用户名的值
> var username = this.value;  // this：给谁绑定的事件，this就代表谁
> ```
>
> 而携带数据需要将URL修改为：
>
> ```js
> xhttp.open("GET", "http://localhost:8080/selectUserServlet?username="+username);
> ```

- **第三步：处理响应-是否显示提示信息**

> 当 `this.readyState == 4 && this.status == 200` 条件满足时，说明已经成功响应数据了。
>
> 此时需要判断响应的数据是否是 "true" 字符串，如果是说明用户名已经占用给出错误提示；如果不是说明用户名未被占用清除错误提示：

```js
// 判断
if(this.responseText == "true"){
    // 用户名存在，显示提示信息
    document.getElementById("username_err").style.display = '';
}else {
    // 用户名不存在 ，清楚提示信息
    document.getElementById("username_err").style.display = 'none';
}
```

- **前端总代码：**

```js
// 1.给用户名输入框绑定 失去焦点事件
document.getElementById("username").onblur = function () {
    // 2.发送ajax请求
    // 获取用户名的值
    var username = this.value;

    // 2.1.创建核心对象
    var xhttp;
    if (window.XMLHttpRequest) {
        xhttp = new XMLHttpRequest();
    } else {
        // code for IE6, IE5
        xhttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
    // 2.2.发送请求
    xhttp.open("GET", "http://localhost:8080/selectUserServlet?username="+username);
    xhttp.send();

    // 2.3.获取响应
    xhttp.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            // alert(this.responseText);
            // 判断
            if(this.responseText == "true"){
                // 用户名存在，显示提示信息
                document.getElementById("username_err").style.display = '';
            }else {
                // 用户名不存在 ，清楚提示信息
                document.getElementById("username_err").style.display = 'none';
            }
        }
    };
}
```

# 【Axios】

> Axios 对原生的AJAX进行封装，简化书写。
>
> Axios官网：[Axios 中文文档 | Axios 中文网 | Axios 是一个基于 promise 的网络请求库，可以用于浏览器和 node.js (axios-http.cn)](https://www.axios-http.cn/)

## 安装

```html
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js">
```

## 基本使用

**使用步骤：**

- **引入axios的js文件**

```html
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js">
```

- **使用axios发送请求，并获取响应结果**

  - 发送get请求

  ```js
  axios({
      method:"get",
      url:"http://localhost:8080/aJAXDemo1?username=zhangsan"
  }).then(function (resp){
      alert(resp.data);
  })
  ```

  - 发送post请求

  ```js
  axios({
      method:"post",
      url:"http://localhost:8080/aJAXDemo1",
      data:"username=zhangsan"
  }).then(function (resp){
      alert(resp.data);
  });
  ```

`axios()`是用来发送异步请求的，小括号中使用js对象传递请求相关的参数：

* `method`属性：用来设置请求方式的，取值为`get`或者`post`。
* `url`属性：用来书写请求的资源路径，如果是`get` 请求，需要将请求参数拼接到路径的后面，格式为：`url?参数名=参数值&参数名2=参数值2`。
* `data`属性：作为请求体被发送的数据，也就是说如果是`post`请求的话，数据需要作为 `data`属性的值。

## 快速入门

### 后端实现

**定义一个用于接收请求的servlet：**

```java
@WebServlet("/axiosServlet")
public class AxiosServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("get...");
        // 1.接收请求参数
        String username = request.getParameter("username");
        System.out.println(username);
        //2.响应数据
        response.getWriter().write("hello Axios~");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("post...");
        this.doGet(request, response);
    }
}
```

### 前端实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
<script>
    // // 1. get
    // axios({
    //      method:"get",
    //      url:"http://localhost:8080/axiosServlet?username=zhangsan"
    //  }).then(function (resp) {
    //      alert(resp.data);
    //  })

    // 2. post  在js中{} 表示一个js对象，而这个js对象中有三个属性
    axios({
        method:"post",
        url:"http://localhost:8080/axiosServlet",
        data:"username=zhangsan"
    }).then(function (resp) {
        alert(resp.data);
    })
</script>
</body>
</html>
```

## 请求方法别名

> 为了方便起见，Axios已经为所有支持的请求方法提供了别名

| 请求类型  |               别名                |
| :-------: | :-------------------------------: |
|   `get`   |     `axios.get(url[,config])`     |
|  `post`   |   `axios.option(url[,config])`    |
| `delete`  |   `axios.delete(url[,config])`    |
|  `head`   |    `axios.head(url[,config])`     |
| `options` |   `axios.option(url[,config])`    |
|   `put`   |  `axios.put(url[,data[,config])`  |
|  `patch`  | `axios.patch(url[,data[,config])` |

**入门案例中的`get`请求代码可改成如下：**

```js
axios.get("http://localhost:8080/axiosServlet?username=zhangsan").then(function (resp) {
    alert(resp.data);
});
```

**入门案例中的`post`请求代码可以改成如下：**

```js
axios.post("http://localhost:8080/axiosServlet","username=zhangsan").then(function (resp) {
    alert(resp.data);
})
```

# 【JSON】

## 概述

> **JSON：JavaScript Object Notation，JavaScript对象表示法**
>
> **作用：由于其语法格式简单，层次结构鲜明，多用于作为数据载体，在网络中进行数据传输。**
>
> **JSON的格式：**
>
> ```json
> {
> 	"name":"zhangsan",
> 	"age":23,
> 	"city":"北京"
> }
> ```

## 基础语法

`JSON` 本质就是一个字符串， 定义格式如下：

```js
var 变量名 = '{"key":value,"key":value,...}';
```

键值之间冒号隔开，键值对之间逗号隔开，最后一个键值对不需要逗号

### 键值对

`JSON` 串的键要求必须使用双引号括起来，而值根据要表示的类型确定。

```java
"key":value
```

例如：

```java
"name":"张三"
```

### 数据类型

value的数据类型分为如下

* 数字（整数或浮点数）
* 字符串（使用双引号`""`括起来）
* 逻辑值（true或者false）
* 数组（在方括号`[]`中）
* 对象（在花括号`{}`中）
* null

### JSON 数字

JSON 数字可以是整型或者浮点型：

```java
{ "age":30 }
```

### JSON 对象

JSON 对象在大括号 **{}** 中书写：

```java
{
    "key1":value1, 
 	"key2":value2, 
 	... 
 	"keyN":valueN 
}
```

对象可以包含多个名称/值对：

```java
{ "name":"菜鸟教程" , "url":"www.runoob.com" }
```

### JSON 数组

JSON 数组在中括号 **[]** 中书写：

数组可包含多个对象：

```json
[
    { key1:value1-1, key2:value1-2 }, 
    { key1:value2-1, key2:value2-2 }, 
    { key1:value3-1, key2:value3-2 }, 
    ...
    { keyN:valueN-1, keyN:valueN-2 }, 
]
```

```java
{
    "sites": [
        { "name":"菜鸟教程" , "url":"www.runoob.com" }, 
        { "name":"google" , "url":"www.google.com" }, 
        { "name":"微博" , "url":"www.weibo.com" }
    ]
}
```

在上面的例子中，对象 **sites** 是包含三个对象的数组。每个对象代表一条关于某个网站（name、url）的记录。

### JSON 布尔值

JSON 布尔值可以是 true 或者 false：

```java
{ "flag":true }
```

------

### JSON null

JSON 可以设置 null 值：

```java
{ "runoob":null }
```

## 后端处理JSON

> 前端发送请求时，如果是复杂的数据就会以JSON提交给后端；而后端如果需要响应一些复杂的数据时，也需要以JSON格式将数据响应回给浏览器。
>
> * 请求数据：JSON字符串转为Java对象
> * 响应数据：Java对象转为JSON字符串
>

> `Fastjson` 是阿里巴巴提供的一个Java语言编写的高性能功能完善的`JSON`库，可以实现`Java`对象和`JSON`字符串的相互转换。

- 引入依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```

- Java对象转JSON对象

```java
String jsonStr = JSON.toJSONString(obj);
```

- JSON对象转Java对象

```java
User user = JSON.parseObject(jsonStr, User.class);
```

## 前端处理JSON

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210830234803335.png)

**后端传递的JSON**

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210831093617083.png" alt="image-20210831093617083" style="zoom:80%;" />

**前端处理代码：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    
<a href="addBrand.html"><input type="button" value="新增"></a>
<br>
<hr>
    
<table id="brandTable" border="1" cellspacing="0" width="100%">
   <!-- 数据添加到这里 -->
</table>
    
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
<script>
	// 1.当页面加载完成后，发送ajax请求
    window.onload = function () {
    // 2. 发送ajax请求
	axios({
    	method:"get",
    	url:"http://localhost:8080/selectAllServlet"
	}).then(function (resp) {
		//获取数据
            let brands = resp.data;
            let tableData = " <tr>\n" +
                "        <th>序号</th>\n" +
                "        <th>品牌名称</th>\n" +
                "        <th>企业名称</th>\n" +
                "        <th>排序</th>\n" +
                "        <th>品牌介绍</th>\n" +
                "        <th>状态</th>\n" +
                "        <th>操作</th>\n" +
                "    </tr>";
            for (let i = 0; i < brands.length ; i++) {
                let brand = brands[i];
                tableData += "\n" +
                    "    <tr align=\"center\">\n" +
                    "        <td>"+(i+1)+"</td>\n" +
                    "        <td>"+brand.brandName+"</td>\n" +
                    "        <td>"+brand.companyName+"</td>\n" +
                    "        <td>"+brand.ordered+"</td>\n" +
                    "        <td>"+brand.description+"</td>\n" +
                    "        <td>"+brand.status+"</td>\n" +
                    "\n" +
                    "        <td><a href=\"#\">修改</a> <a href=\"#\">删除</a></td>\n" +
                    "    </tr>";
                /*
                	tableData拼接如下：
                	表头
                	<tr> 
                       	<th>序号</th>
                       	<th>品牌名称</th>
                       	<th>企业名称</th>
                       	<th>排序</th>
                       	<th>品牌介绍</th>
                       	<th>状态</th>
                       	<th>操作</th>
                    </tr>
                	接下来的每一行都是这样的格式
               		<tr align="center">
               			<td>序号1</td>
               			<td>品牌名称1</td>
               			<td>企业名称1</td>
               			<td>排序1</td>
               			<td>品牌介绍1</td>
               			<td>状态1</td>
               			<td>
               				<a href="#">修改</a> 
               				<a href="#">删除</a>
    					</td>
               		</tr>
               		...
                */
               
            }
            // 添加表格的html代码
            document.getElementById("brandTable").innerHTML = tableData;
	});
}
</script>
```

# 【Vue】

> [Vue.js (vuejs.org)](https://cn.vuejs.org/v2/guide/)

## 简介

> Vue是一套前端框架，免除原生JavaScript中的DOM操作，简化书写。
>
> Vue基于MVVM(Model-View-ViewModel)思想，实现数据的双向绑定，将编程的关注点放在数据上：
>
> - MVC思想
>
> <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210831113940588.png" alt="image-20210831113940588" style="zoom:70%;" />
>
> ​	C是js代码，M是数据，V是页面上展示的内容
>
> ​	MVC思想没法进行双向绑定。双向绑定是指当数据模型数据发生变化时，页面展示的会随之发生变化，而如果表单数据发生变化，绑定的模型数据也随之发生变化。
>
> - MVVM思想
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210831114805052.png)
>
> ​	图中的`Model`是数据，`View`是视图
>
> ​	`Model`和`View`是通过`ViewModel`对象进行双向绑定的，而`ViewModel`对象是 `Vue` 提供的。

## 安装

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
```

## 快速入门

**Vue操作步骤：**

**1.引入Vue.js文件**

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```

**2.在JS代码区域，创建Vue核心对象，进行数据绑定**

```js
new Vue({
    el: "#app",
    data() {
        return {
            username:""
        }
    }
});
```

​	创建 Vue 对象时，需要传递一个js对象，而该对象中需要如下属性：

* `el`：用来指定哪些标签受Vue管理。该属性取值`#app`中的`app`需要是受管理的标签的id属性值
* `data`：用来定义数据模型
* `methods`：用来定义函数

**3.编写视图**

```html
<div id="app">
    <input name="username" v-model="username" >
    {{username}}
</div>
```

`{{}}`是Vue中定义的`插值表达式`，在里面写数据模型，到时候会将该模型的数据值展示在这个位置。

**全部代码：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <input v-model="username">
    <!--插值表达式-->
    {{username}}
</div>
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
    // 1.创建Vue核心对象
    new Vue({
        el:"#app",
        data(){  // data() 是 ECMAScript 6 版本的新的写法
            return {
                username:""
            }
        }
</script>
</body>
</html>
```

## Vue指令

> **指令：**HTML 标签上带有v-前缀的特殊属性，不同指令具有不同含义

**常用的指令：**

|  **指令**   |                      **作用**                       |
| :---------: | :-------------------------------------------------: |
|  `v-bind`   |    为HTML标签绑定属性值，如设置href , css样式等     |
|  `v-model`  |            在表单元素上创建双向数据绑定             |
|   `v-on`    |                 为HTML标签绑定事件                  |
|   `v-if`    |                                                     |
|  `v-else`   |   条件性的渲染某元素，判定为true时渲染,否则不渲染   |
| `v-else-if` |                                                     |
|  `v-show`   | 根据条件展示某元素，区别在于切换的是display属性的值 |
|   `v-for`   |       列表渲染，遍历容器的元素或者对象的属性        |

### v-bind和v-model

* **v-bind**

  该指令可以给标签原有属性绑定模型数据

  这样模型数据发生变化，标签属性值也随之发生变化

  例如：

  ```html
  <a v-bind:href="url">百度一下</a>
  <!-- v-bind:  可以简化写成: -->
  <a :href="url">点击一下</a>
  ```

* **v-model**

  该指令可以给表单项标签绑定模型数据。这样就能实现双向绑定效果

  ```html
  <input name="username" v-model="username">
  ```

**综合**

```html
<div id="app">
    <!-- href的值根据url动态变化 -->
    <a v-bind:href="url">点击一下</a>
    <!-- url的值会根据input输入框中输入的地址动态变化 -->
    <input v-model="url">
</div>
    
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data(){
            return {
                username:"",
                url:"https://www.baidu.com"
            }
        }
    });
</script>
```

### v-on

`v-on`可以为HTML标签绑定事件

```html
<div id="app">
    <!-- 为单击事件绑定了show方法 -->
    <input type="button" value="一个按钮" v-on:click="show()">
    <br>
    <!-- 简写 -->
    <input type="button" value="一个按钮" @click="show()">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
    //1. 创建Vue核心对象
    new Vue({
        el:"#app",
        data(){
            return {
                username:"",
            }
        },
        methods:{
            show(){
                alert("我被点了...");
            }
        }
    });
</script>
```

> `v-on:` 后面的事件名称是之前原生事件属性名去掉on
>
> 例如：
>
> * 单击事件：事件属性名是onclick，而在vue中使用是 `v-on:click`
> * 失去焦点事件：事件属性名是onblur，而在vue中使用时 `v-on:blur`

### 条件判断指令

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210831151904081.png" alt="image-20210831151904081" style="zoom:70%;" />

```html
<div id="app">
    <!-- 根据count值不同展示不同的div -->
    <div v-if="count == 3">div1</div>
    <div v-else-if="count == 4">div2</div>
    <div v-else>div3</div>
    <!-- v-show用法和v-if一致 -->
    <div v-show="count == 3">div1</div>
    <hr>
    <!-- count根据input的值动态变化 -->
    <input v-model="count">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data(){
            return {
                count:3
            }
        }
    });
</script>
```

> `v-show` 不展示的原理是给对应的标签添加`display`css属性，并将该属性值设置为 `none` ，这样就达到了隐藏的效果。
>
> 而 `v-if` 指令是条件不满足时根本就不会渲染。

### v-for

`v-for` 用于列表渲染，会遍历容器的元素或者对象的属性

**格式：**

> 需要循环那个标签，`v-for`指令就写在那个标签上

```html
<标签 v-for="变量名 in 集合模型数据">
    {{变量名}}
</标签>

<!-- 如果在页面需要使用到集合模型数据的索引，就需要使用如下格式： -->
<标签 v-for="(变量名,索引) in 集合模型数据">
    <!-- 索引变量是从0开始，所以要表示序号的话，需要手动的加1 -->
    <!-- 展示索引 -->
    {{索引 + 1}}
    <!-- 展示值 -->
    {{变量名}}
</标签>
```

**例如：**

```html
<div id="app">
    <div v-for="addr in addrs">
        {{addr}}
    </div>

    <hr>
    <div v-for="(addr,i) in addrs">
        {{i+1}}--{{addr}}
    </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
    new Vue({
        el:"#app",
        data(){
            return {
                addrs:["北京","上海","西安"]
            }
        }
    });
</script>
```

**效果：**

![image-20220515201642424](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220515201642424.png)

## 生命周期

> Vue的生命周期有八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法，这些生命周期方法也被称为钩子方法
>
> <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210831160239294.png" alt="image-20210831160239294" style="zoom:80%;" />
>
> Vue官网提供的从创建Vue到效果Vue对象的整个过程及各个阶段对应的钩子函数：
>
> <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210831160335496.png" alt="image-20210831160335496" style="zoom:80%;" />
>
> `mounted`：挂载完成，Vue初始化成功，HTML页面渲染成功，可以在该方法中发送异步请求，加载数据

**Vue发送异步请求：**

```js
new Vue({
    el: "#app",
    data(){
        return{
            brands:[]
        }
    },
    mounted(){
        // 页面加载完成后，发送异步请求，查询数据
        var _this = this;
        axios({
            method:"get",
            url:"http://localhost:8080/brand-demo/selectAllServlet"
        }).then(function (resp) {
            _this.brands = resp.data;
        })
    }
})
```

**Vue视图：**

```html
<!-- 定义<div id="app"></div>，指定该div标签受Vue管理 -->
<div id="app">
    <a href="addBrand.html"><input type="button" value="新增"></a><br>
    <hr>
    <table id="brandTable" border="1" cellspacing="0" width="100%">
        <tr>
            <th>序号</th>
            <th>品牌名称</th>
            <th>企业名称</th>
            <th>排序</th>
            <th>品牌介绍</th>
            <th>状态</th>
            <th>操作</th>
        </tr>
        <!-- 使用v-for遍历tr -->
        <tr v-for="(brand,i) in brands" align="center">
            <td>{{i + 1}}</td>
            <td>{{brand.brandName}}</td>
            <td>{{brand.companyName}}</td>
            <td>{{brand.ordered}}</td>
            <td>{{brand.description}}</td>
            <td>{{brand.statusStr}}</td>
            <td><a href="#">修改</a> <a href="#">删除</a></td>
        </tr>
    </table>
</div>
```

# 【Element-UI】

> [Element - 网站快速成型工具](https://element.eleme.cn/#/zh-CN)

## 简介

> Element-UI 是一套采用 Vue 2.0 作为基础框架实现的组件库，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的组件库，提供了配套设计资源，帮助网站快速成型
>
> 学习Element其实就是学习怎么从官网拷贝组件到自己的页面并进行修改

## 安装

**CDN引入：**

```html
<!-- 引入Vue -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
```

**本地引入：**

element-ui放到webapp文件夹下

```html
<!-- 引入组件库 -->
<script src="element-ui/lib/index.js"></script>
<!-- 引入样式 -->
<link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
```



## 快速入门

- **创建页面，并在页面中引入Element的样式和组件库**

```html
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
```

- **创建Vue核心对象**

```html
<!-- Element是基于Vue的，所以使用Element时必须要创建Vue对象 -->
<script>
    new Vue({
        el:"#app"
    })
</script>
```

- **官网复制Element组件代码**

```html
<el-row>
    <el-button>默认按钮</el-button>
    <el-button type="primary">主要按钮</el-button>
    <el-button type="success">成功按钮</el-button>
    <el-button type="info">信息按钮</el-button>
    <el-button type="warning">警告按钮</el-button>
    <el-button type="danger">危险按钮</el-button>
</el-row>
<el-row>
    <el-button plain>朴素按钮</el-button>
    <el-button type="primary" plain>主要按钮</el-button>
    <el-button type="success" plain>成功按钮</el-button>
    <el-button type="info" plain>信息按钮</el-button>
    <el-button type="warning" plain>警告按钮</el-button>
    <el-button type="danger" plain>危险按钮</el-button>
</el-row>
<el-row>
    <el-button round>圆角按钮</el-button>
    <el-button type="primary" round>主要按钮</el-button>
    <el-button type="success" round>成功按钮</el-button>
    <el-button type="info" round>信息按钮</el-button>
    <el-button type="warning" round>警告按钮</el-button>
    <el-button type="danger" round>危险按钮</el-button>
</el-row>
<el-row>
    <el-button icon="el-icon-search" circle></el-button>
    <el-button type="primary" icon="el-icon-edit" circle></el-button>
    <el-button type="success" icon="el-icon-check" circle></el-button>
    <el-button type="info" icon="el-icon-message" circle></el-button>
    <el-button type="warning" icon="el-icon-star-off" circle></el-button>
    <el-button type="danger" icon="el-icon-delete" circle></el-button>
</el-row>
```

**全部代码：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <el-row>
        <el-button>默认按钮</el-button>
        <el-button type="primary">主要按钮</el-button>
        <el-button type="success">成功按钮</el-button>
        <el-button type="info">信息按钮</el-button>
        <el-button type="warning">警告按钮</el-button>
        <el-button type="danger">危险按钮</el-button>
    </el-row>

    <el-row>
        <el-button plain>朴素按钮</el-button>
        <el-button type="primary" plain>主要按钮</el-button>
        <el-button type="success" plain>成功按钮</el-button>
        <el-button type="info" plain>信息按钮</el-button>
        <el-button type="warning" plain>警告按钮</el-button>
        <el-button type="danger" plain>危险按钮</el-button>
    </el-row>

    <el-row>
        <el-button round>圆角按钮</el-button>
        <el-button type="primary" round>主要按钮</el-button>
        <el-button type="success" round>成功按钮</el-button>
        <el-button type="info" round>信息按钮</el-button>
        <el-button type="warning" round>警告按钮</el-button>
        <el-button type="danger" round>危险按钮</el-button>
    </el-row>

    <el-row>
        <el-button icon="el-icon-search" circle></el-button>
        <el-button type="primary" icon="el-icon-edit" circle></el-button>
        <el-button type="success" icon="el-icon-check" circle></el-button>
        <el-button type="info" icon="el-icon-message" circle></el-button>
        <el-button type="warning" icon="el-icon-star-off" circle></el-button>
        <el-button type="danger" icon="el-icon-delete" circle></el-button>
    </el-row>
</div>
<!-- 引入Vue -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<!-- 引入组件库 -->
<script src="element-ui/lib/index.js"></script>
<!-- 引入样式 -->
<link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
<!-- Vue管理 -->
<script>
    new Vue({
        el:"#app"
    })
</script>
</body>
</html>
```

**效果：**

![image-20220515204945584](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220515204945584.png)

## 布局

> Element 提供了两种布局方式，分别是：
>
> * Layout 布局
> * Container 布局容器

### Layout布局

> 通过基础的24分栏，迅速简便地创建布局。也就是默认将一行分为 24 栏，根据页面要求给每一列设置所占的栏数。

**官方代码：**

![image-20220515205253953](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220515205253953.png)

```html
<!-- html代码 -->
<!-- 一行占24个格子 -->
<el-row>
    <!-- bg-purple-dark：暗 -->
  <el-col :span="24"><div class="grid-content bg-purple-dark"></div></el-col>
</el-row>
<!-- 一行分两块，每块占12个格子 -->
<el-row>
    <!-- bg-purple：正常 -->
  <el-col :span="12"><div class="grid-content bg-purple"></div></el-col>
  	<!-- bg-purple-light：亮 -->
    <el-col :span="12"><div class="grid-content bg-purple-light"></div></el-col>
</el-row>
<!-- 一行占分三块，每块占8个格子 -->
<el-row>
  <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="8"><div class="grid-content bg-purple-light"></div></el-col>
  <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
</el-row>
<el-row>
  <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="6"><div class="grid-content bg-purple-light"></div></el-col>
  <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="6"><div class="grid-content bg-purple-light"></div></el-col>
</el-row>
<el-row>
  <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
  <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
  <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
</el-row>

<!-- css样式 -->
<style>
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>
```

> html代码放到`<body>`标签
>
> style代码放到`<head>`标签

### Container 布局容器

> 用于布局的容器组件，方便快速搭建页面的基本结构：
>
> `<el-container>`：外层容器。当子元素中包含 `<el-header>` 或 `<el-footer>` 时，全部子元素会垂直上下排列，否则会水平左右排列。
>
> `<el-header>`：顶栏容器。
>
> `<el-aside>`：侧边栏容器。
>
> `<el-main>`：主要区域容器。
>
> `<el-footer>`：底栏容器。

**官方实例：**

```html
<!-- 页面 -->
<el-container style="height: 500px; border: 1px solid #eee">
  <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
    <el-menu :default-openeds="['1', '3']">
      <el-submenu index="1">
        <template slot="title"><i class="el-icon-message"></i>导航一</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="1-1">选项1</el-menu-item>
          <el-menu-item index="1-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="1-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="1-4">
          <template slot="title">选项4</template>
          <el-menu-item index="1-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-submenu index="2">
        <template slot="title"><i class="el-icon-menu"></i>导航二</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="2-1">选项1</el-menu-item>
          <el-menu-item index="2-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="2-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="2-4">
          <template slot="title">选项4</template>
          <el-menu-item index="2-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-submenu index="3">
        <template slot="title"><i class="el-icon-setting"></i>导航三</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="3-1">选项1</el-menu-item>
          <el-menu-item index="3-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="3-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="3-4">
          <template slot="title">选项4</template>
          <el-menu-item index="3-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
    </el-menu>
  </el-aside>
  
  <el-container>
    <el-header style="text-align: right; font-size: 12px">
      <el-dropdown>
        <i class="el-icon-setting" style="margin-right: 15px"></i>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>查看</el-dropdown-item>
          <el-dropdown-item>新增</el-dropdown-item>
          <el-dropdown-item>删除</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <span>王小虎</span>
    </el-header>
    
    <el-main>
      <el-table :data="tableData">
        <el-table-column prop="date" label="日期" width="140">
        </el-table-column>
        <el-table-column prop="name" label="姓名" width="120">
        </el-table-column>
        <el-table-column prop="address" label="地址">
        </el-table-column>
      </el-table>
    </el-main>
  </el-container>
</el-container>

<!-- css样式 -->
<style>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
</style>

<!-- vue -->
<script>
  export default {
    data() {
      const item = {
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      };
      return {
        tableData: Array(20).fill(item)
      }
    }
  };
</script>
```

