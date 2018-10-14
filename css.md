# CSS

## 规范部分

### CSS 属性定义

![css-property](https://raw.githubusercontent.com/temple-deng/markdown-images/master/css/css-property.png)   

**Value**    

value 部分指定了属性的有效值，一个属性值可能包括几部分，每部分的值的类型可能按照多种方式设设计的：  

1. 关键字值，例如 auto
2. 基础数据类型，即出现 &lt; 和 &gt; 的内容，例如 &lt;length&gt;
3. 与具有相同名称的属性具有相同值范围的类型，例如 &lt;'border-width'&gt;,
&lt;'background-attachment'&gt;。
4. non-terminals that do not share the same name as a property. In this case, 
the non-terminal name appears between "&lt;" and "&gt;", as in &lt;border-width&gt;.
Notice the distinction between &lt;border-width&gt; and &lt;'border-width'&gt;;
the latter is defined in terms of the former. The definition of a non-terminal is
located near its first appearance in the specification.     

不同部分的值可能按如下方式排列到属性值中（优先级从上到下）：   

+ 几个并列的单词意味着所有这些单词必须以给定的顺序出现
+ 竖线 | 分隔的两个或多个选项，必须且只能出现一个
+ 双竖线 || 分隔的两个或多个选项，可以以任意顺序出现一到多个
+ 双 && 分隔的两个或多个选项，所有选项必须都出现，不过可以以任意顺序
+ 方括号 [] 用于分组   

每个部分值可能还跟着重复次数标识符：    

+ * 表示出现 0 或多次
+ + 表示出现 1 或多次
+ ? 表示出现 0 或 1 次
+ {A,B} 出现 A-B 次    

### 语法和基础数据类型

#### 关键字

关键字不能用引号包起来。   

#### 特性和例子

+ 所有在 ASCII 范围内的 CSS 语法字符都是大小写不敏感的，除了那些不受 CSS 控制的部分，例如 HTML
元素的 id 和 class，字体名称，URI等。
+ CSS 标识符（包括选择器中的元素名称，类，id）可以包含字符 [a-zA-Z0-9] 已经 ISO 10646 中
U+0080 及以上的字符，外加下划线 _ 和中划线 -。不过不可以使用数字，双中划线，或者一条中划线后
跟一个数字开头。可以包括转义字符。注意 ISO 10646 和 Unicode 码点是等价的。    

#### 声明

一个 CSS 样式表通常包括一系列声明，这些声明可以分为两类：_at-rules_ 和 _rule sets_。   

#### At-rules

at-rules 以一个 @ 字符开头，后面紧跟一个标识符，例如 @import, @page。   

一条 at-rule 声明的内容到一个分号或下一个声明块截止。   

@import 规则必须在样式表的头部，其前面只能出现 @import 和 @charset 规则。这种情况下，不合法
位置的 @import 规则理应被浏览器忽略掉。    

#### Rule sets

一个 rule set 包括一个选择器和一个声明块。声明块中可以包含零到多条声明和 at-rules 声明。最后
一条声明后面可以不加分号。CSS2 中是没有任何 at-rules 是可以内嵌到 rule sets 中的。    

注意选择器部分是可以使用逗号分隔多个选择器的。    

#### 值

1. 整型和实数 &lt;integer&gt;, &lt;number&gt;
2. 长度 &lt;length&gt;，长度值的格式通常是一个数字 &lt;number&gt; 后面跟着一个单位标识符，
如果是 0 宽的话，单位可以省略。
  - 相对单位包括：
    + em：相对于 font-szie
    + ex：相对于字体的 x-height，x-height 是等于小写 x 的高度
  - 绝对单位：
    + in：英尺
    + cm：里面
    + mm：毫米
    + pt：1/72 英尺
    + pc：12pt
    + px：0.75pt
3. 百分比
4. uri，uri 的括号两边可以加可选的空格，地址也可以可选地使用单引号或双引号包裹起来
5. 计数器 counters，计算器由大小写敏感的标识符表示，为了引用计数器的值，使用 counter(&lt;identifier&gt;)
或 counter(&lt;identifier&gt;,&lt;'list-style-type'&gt;)，并使用可选的空格分隔标记
6. 颜色
7. 字符串     

### 选择器

#### CSS2.2 可用的选择器

+ * 通用选择器
+ E 类型选择器
+ E F 后代选择器
+ E &gt; F 子选择器
+ E:first-child 第一孩子伪类
+ E:link（未访问过），E:visited（已访问过），链接伪类
+ E:active, E:hover, E:focus 动态伪类
+ E:lang(c)
+ E + F 邻接选择
+ E[foo] 属性选择器
+ E[foo="warning"] 属性选择
+ E[foo~="warning"] foo 属性是一个空格分隔的列表，其中有一个正好等于 warning
+ E[lang|="en"] lang 的值恰好是 en 或者是一个 *-en 的形式
+ class
+ id    

#### 伪类和伪元素

+ 伪元素创建了关于文档树的一种抽象，这种抽象超越了文档语言的能力。例如，文档语言不提供访问第一行
或第一个字符的能力。CSS 伪元素允许我们引用这种无法访问的信息。伪元素还提供了一种方式让我们给文档
中不存在的内容提供样式的。
+ 伪类根据对名称，属性或内容以外的特性对元素进行分类，这些特性是一般无法通过文档树得出的。   

### 属性赋值，级联和继承

一个属性值的计算过程分了四步：首先是提取出声明值，之后处理成一个可以用来继承的计算值，之后如果
有必要的话转换为一个绝对的使用值，最后根据环境的限制决定是否要转换为一个实际使用值。    

#### 声明值 specified values

首先要按照如下机制给每个属性得出一个声明值：   

1. 如果级联的结果是一个值的话就直接使用，如果是 inherit，就使用父元素的计算值
2. 如果属性是可继承的，元素也不是文档根节点，使用父元素的计算值
3. 否则使用属性初始值    

### 盒模型

1. 非置换内联元素的垂直方向上的 margin 无效果
2. 百分比的 margin 是根据生成盒的包含块宽度计算的，即便是垂直方向上的 top 和 bottom 也是，百分比
的 padding 也是
3. 一个属性是否可继承和其值能否为 inherit 是没有关系的，是否可继承是这个元素默认会不会被子元素
继承，而 inherit 属性值是强制使用父元素的计算值作为声明值
4. padding 不可取 auto 和负值   

### 视觉格式化模型

1. 在视觉格式化模型中，文档树中的每个元素都会根据盒模型生成零或多个盒子，这些盒子的排列受以下
因素影响：
  + 盒子的尺寸和类型
  + 定位模式（常规流、浮动、绝对定位）
  + 元素间的相对关系
  + 外部信息（视口尺寸，图片的内在尺寸）
2. 许多盒子的位置和大小都是根据一个叫做 **包含块** 的矩形盒子计算的。一般来说，一个元素的生成盒子
扮演了后代盒子的包含块的角色。也可以说一个盒子为其后代建立的包含块。
3. display 属性指定了一个盒子的类型。这个属性控制的是元素的 **主盒子**，这个主盒子就是包含其
生成的内容和后代盒子，以及参与定位模式的盒子。向 list-item 元素可能生成除主盒子之外的盒子。
4. 块级元素生成块级主盒子，block, list-item, table 也可以让一个元素成为块级盒子。块级盒子
参与 BFC。
5. 除 table 和置换元素的主盒子之外的块级盒子也是块容器盒。块容器盒要么只包括块级盒子，要么
建立 IFC 只包含内联级盒子。
6. block, list-item, inline-block 也可以让一个非置换元素生成块容器盒。
7. 非置换的 inline blocks 和 table cells 是块容器盒，但不是块级盒子。
8. inline, inline-table, inline-block 可以使元素成为一个行内级元素。行内级元素生成行内级
盒子，参与 IFC。
9. display: inline 的非置换元素会生成一个 inline box。非 inline box 的行内级盒子都是原子
行内级盒子，因为其参与 IFC 的方式是其内容作为一个整体的单独盒子，而 inline box 的参与 IFC 的
方式可能是以多个分离的盒子。
10. 在 CSS2.2 中，一个盒子可能根据以下 3 种定位模式进行布局：
  - 常规流，包括块级盒的 BFC，行内级盒子的 IFC，以及块级盒和行内级盒子的相对定位
  - 浮动。首先根据常规流将盒子进行摆放，然后把它从常规流里取出来，往左边或右边尽可能地移动。内容
  可能沿着浮动的边缘进行排列。
  - 绝对定位。整个从常规流中将盒子取出，根据其包含块安排新的位置   
11. 一个元素如果是浮动、绝对定位或者是根元素，那么元素就称为 **流外的**out of flow。否则
就是流内的 in-flow。
12. OK，所以首先介绍了元素是生成盒子来参与布局的，然后介绍了不同元素生成的不同的种类的盒子，然后
介绍了这些盒子进行布局时不同的布局模式（常规流，浮动，绝对定位），而常规流内的布局又可以分为几种
不同格式化上下文，或者说布局上下文。
13. 常规流内的盒子都属于一个格式化上下文，在 CSS 中可能是 table, block, inline。
14. 注意参与格式化上下文布局，与建立一个格式化上下文之间的区别，看情况，参与格式化上下文的元素
就像上面说的，是在常规流内的，符合常规流的布局模式，但是格式化上下文总要有元素建立把，这些建立
格式化上下文的元素可不一定是在常规流内的哦。
15. 浮动、绝对定位元素、非块盒子的块容器，以及 overflow 不为 visible 的块盒子会为其内容建立
新的 BFC。
16. 在 BFC 中，垂直方向上盒子从上向下排列，margin 可能折叠，水平方向上从包含块左边缘开始，
即便有浮动元素也是一样，不过 line box 也能会变小。
17. 一个不包含块级盒子的块容器盒建立一个 IFC。IFC 中的盒子可能在垂直方向上根据不同的方式进行
对齐：根据 bottom 或 top 对齐，或者基于文本的基线对齐。
18. 一个浮动的盒子向左或右移动直到遇到包含块的边缘或者其他浮动盒子的边缘。如果水平距离不够，就下移。
19. 根据实验来看，如果水平方向放不下浮动元素，那么只要向下移到前一个浮动元素的下面进行摆放就行，
当然如果前一个下方剩下的距离不够就继续往下。而不是选择所有之前浮动元素的最下方。
20. 块级置换元素或者建立了新的 BFC 的元素的 margin box 与同一 BFC 中的浮动的 margin 不可
重叠。
21. 对于一个定位的盒子（这里的定位包括相对定位），z-index 属性指明：
  - 盒子在当前层叠上下文中的层叠水平 level
  - 是否盒子建立了层叠上下文   
22. 一个整型的 z-index 指明了当前盒子在当前层叠上下文中的层叠水平的同时，也建立了一个新的层叠
上下文。如果是 auto，同时盒子是 fixed 定位或者是根元素，那也会建立新的层叠上下文。
23. 在每个层叠上下文中，下面的各层从底向上绘制：
  1. 形成层叠上下文的元素的背景和边框
  2. 负层叠水平的子层叠上下文元素
  3. 流内，非内联，非定位的后代
  4. 非定位的浮动
  5. 流内，内联非定位后代
  6. 0 层叠水平的子层叠上下文，和 0 层叠水平的定位后代(这种是声明成 auto 了把)
  7. 正层叠水平的子层叠上下文

### 格式化上下文的细节

1. 包含块的定义：
  1. 根元素所在的包含块叫初始包含块。
  2. 对于 relative, static 定位的元素，包含块是最近的块容器盒祖先或者建立了格式化上下文
  的祖先的内容区域形成的盒子
  3. fixed，是视口
  4. absolute，由最近的 absolute, relative, fixed 的祖先按照如下方式生成：
    - 如果祖先是行内元素，包含块是元素第一行到最后一行的 padding boxes 形成的 bounding box，
    - 否则就是祖先的 padding edge 形成的
    - 没有这样的祖先就是初始包含块
2. width 不能适用在非置换内联元素上，正常情况下，百分比是相对于包含块的宽度计算的，不过对于
那些绝对定位的元素，且其包含块是一个块容器元素，这时候是相对于 padding box 计算的。
3. 高度也不能适用在非置换内联元素
4. 非置换内联元素的垂直方向上的 padding, border 是从内容区域的顶部开始的，这些属性和行高
无关，但是计算 line box 的高度时是 line-height 参与计算的，而且注意，这些 border 和 padding
和行间距一样也是均分在行高上下的。
5. clip 是相对于 border box 指定尺寸的

### 媒体查询

1. 注意媒体查询语法在 link 和 @import 中也能用来指定 `import url(color.css) screen and (color)`
2. all 和后面的 and 是可以省略的
3. not 关键词，逗号分隔列表
4. 媒体类型和媒体特征
5. min, max 都含有 equal 的意思的
6. 特征
  - width, height
  - device-width, device-height
  - orientation: portrait | landscape
  - aspect-ratio, device-aspect-ratio 单位 dpi dpcm
  - color, color-index
  - monochrome
  - resolution, scan, grid

### 字体

1. font-family 逗号分隔列表
2. 通用字体族 serif, sans-serif, cursive, fantasy, monospace，通用的都是关键字，不能加
引号
2. 其他字体族的名称也可以不加引号，但好像如果有特殊字符的话就必须加引号作为字符串使用
3. @font-face 至少要包含 font-family 和 src 两个属性，src 也是逗号分隔
4. src 中的字体地址后面可以跟一个提示字体格式的字符串：    

```css
@font-face {
  font-family: bodytext;
  src: url(ideal-sans-serif.woff2) format("woff2"),
      url(good-sans-serif.woff) format("woff"),
      url(basic-sans-serif.ttf) format("opentype");
}
```   

5. local()


### 选择器 3

新加的选择器：   

+ E[foo^="bar"] bar开头
+ E[foo$="bar"] bar 结尾
+ E[foo*="bar"] 包含子字符串 bar
+ E:root
+ E:nth-child(n), E:nth-last-child(n)
+ E:nth-of-type(n) E:nth-last-of-type(n)
+ E:first-of-type, E:last-of-type
+ E:only-child, E:only-of-type
+ E:empty, E:enabled, E:disabled, E:checked

### 值和单位 3

1. 视口相对百分比单位 vw, vh, vmin, vmax

### 背景和边框 3

1. 背景列表也是逗号分隔
2. 背景里面不够的值，通过重复来填充
3. 背景的各属性：
  - background-color，根据 background-clip 剪裁，注意颜色只能有一个，但图片可以有多个
  - background-image
  - background-repeat, repeat-x repeat-y, repeat, no-repeat, space, round, space
  相当于 space-between，round 会缩放
  - background-attachment, fixed, scroll, local
    + fixed: 背景相对于视口是固定的，注意是视口哦
    + local: 背景相对于元素的内容 content 是固定的 fixed，如果元素能滚动，那背景也会随着背景
    滚动，这种就是背景图会上下动的那种
    + scroll: 背景相对于元素本身固定，并且不随着元素滚动，类似 fixed 不动，但是是相对于元素
  - background-position: 百分比是相对于背景定位区域的大小减去背景图片的大小 bottom 10px right 20px
  - background-clip: 背景的 **绘制区域**
  - background-origin: 背景的 **定位区域**
  - background-size:
    + contain: 缩放图片，但保持内部比例，缩放到什么程度呢，宽高同时能放置到背景定位区域的最大尺寸
    + cover: 缩放图片，保持内部比例，缩放到宽高正好能盖住整个背景定位区域的最小尺寸
4. border image，注意边框图片不会影响盒子本身的布局，盒子本身的 border box 还是由 border-width定的：
  - border-image-source
  - border-image-slice: fill，百分比是参考于图片的大小，fill 会造成中间的那块区域不被丢弃
  - border-image-width
  - border-image-outset
  - border-image-repeat: stretch, repeat, round, space

### Text 3

1. text-transform: none | capitalize | uppercase | lowercase | full-width，适用于
inline boxes。


#### white-space

+ 可取值: normal | pre | nowrap | pre-wrap | break-spaces | pre-line
+ 适用于 inline boxes    

这个属性指定了两件事：   

+ 元素内的空白是否以及如何折叠
+ 行在一些非强制换行机会下是否要换行     

各取指含义：  

+ normal: 将多空格序列折叠成一个空格，以及在某些情况下直接都删除掉。在 soft wrap 机会下可以换行。
+ pre: 保留所有空格序列和换行
+ nowrap: 折叠空格序列，但不会自动换行，也就是会保留强制换行符
+ pre-wrap: 保留空格序列，但会自动换行
+ break-spaces: 等同于 pre-wrap，但是有两点不同：第一点是行末的空白序列也要占据空间，第二点
是每个被保留的空白序列后都有一个换行的机会
+ pre-line: 折叠空白序列也允许换行，但是会保留强制断行符。     

这里就有要注意的点，首先可以换行和强制换行是不一样的，可以换行是指我们这个 line box 放不下后面
的内容，那内容是否可以放到下一个 line box 中，而强制换行是指文档树中的换行符。    

![white-space](https://raw.githubusercontent.com/temple-deng/markdown-images/master/css/white-space.png)   

#### 空白处理的细节    

这个东西很复杂啊，因为涉及到了 HTML 对文档中空白的处理，以及 CSS 对空白的处理，针对 HTML 对
空白的处理，了解的不多啊，只知道那些匿名的只包含可折叠空白的块会从渲染树中整体删除。    

CSS 中的空白处理只影响这些空白字符：空格(U+0020), tabs(U+0009) 和 segment breaks 即换行。    

首先第一阶段：折叠和转换    

对 IFC 内的每个 inline box，空白字符是这样处理的：   

+ 如果 white-space 是 normal, nowrap, pre-line，那么就认为空白是可以折叠的，然后按照以下
步骤进行处理：
  1. 换行前后的紧跟的所有空格和 tab 都删除
  2. 根据下面讲到的换行处理规则转换换行符
  3. 将所有 tab 转换为空格
  4. 将跟在另一个可折叠空格后的所有空格都折叠成 0 宽，也就相当于都删除掉只留前面那一个。
+ 如果 white-space 是 pre, pre-wrap, break-spaces，所有空白序列都被当成是一个不可打断的
空格序列。   

**换行转换规则**    

当 white-space 是 pre, pre-wrap, break-spaces, pre-line 时，换行是不可折叠的，相反会
转换成一个应当保留的换行符。    

对于其他的 white-space 值，换行是可折叠的。与空格一致，多换行符会合并成一个。剩下的这一个还会
根据上下文来决定是要转换成空格还是直接删除掉：    

+ 如果这个剩下的换行符前或者后的字符是一个零宽的空白符(U+200B)，那么就可以删除掉这个换行
+ 否则可能要转换为空格（这里说可能是因为规范中还有其他的情况要删除掉换行，但我看不懂，哈哈哈）   

第二阶段，删除和定位：    

+ 每一行开头的和结尾的可折叠空白序列被删除   

#### 断行和单词边界

当一行由于明确的断行控制打断，或者是由于处于一个块的开头或结尾断行，这样的情况称为 **强制断行**
(forced line break)。当一行由于内容需要换行被打断，这样的情况的 **软换行** (soft wrap break)   

换行只会在一个允许打断的点才会执行，这样的位置叫做 **软换行机会**(soft wrap opportunity)。
当这样的换行是允许的情况下，浏览器应该正确地在这样的地方进行换行，以便使一行中溢出的内容最小。   

**字符折断规则：word-break**   

word-break: normal | keep-all | break-all    

这个属性指定了字符间的软换行位置，注意是字符间，空格和其他单词分隔符的控制由 line-break 属性
确定。    

+ normal：单词按照自己的规则进行折断
+ break-all：除了常规的软换行位置外，允许在一个单词中进行折断
+ keep-all：禁止在单词中进行中断，这种情况下，CJK 字符也不能中断。这个应该是类似于 normal，
但是 CJK 字符不能中断了    

**断行规则 line-break**   

这个属性指定了应用在元素上的换行规则的严格性：特别是符号与换行的交互。   

+ auto
+ loose：使用最低的限制来控制断行规则
+ normal
+ strict
+ anywhere: 好像是可以随便断行    

**溢出换行：overflow-wrap/word-wrap**     

这个属性指定了浏览器是否可以在行内不允许中断的点处进行中断而防止溢出。   

+ normal：行只能在允许断行点进行断行
+ break-word：如果行中没有其他可接受的断点，则可以在任意处进行断行。   


## CSS 面试课程

### 1 html 基础

1. colspan 和 rowspan    

```html
<table border="1">
  <thead>
    <tr>
      <th>表头1</th>
      <th>表头2</th>
      <th>表头3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>数据1</td>
      <td>数据2</td>
      <td>数据3</td>
    </tr>
    <tr>
      <td colspan="2">数据1和2</td>
      <td>数据3</td>
    </tr>
    <tr>
      <td rowspan="2">数据1和1</td>
      <td>数据2</td>
      <td>数据3</td>
    </tr>
    <tr>
      <td>数据2</td>
      <td>数据3</td>
    </tr>
  </tbody>
</table>
```    

2. HTML 元素嵌套关系
  + 块级元素可以包含行内元素
  + 块级元素不一定能包含块级元素（p 不能嵌套 div）
  + 行内元素一般不能包含块级元素
3. 语义化的意义
  - 开发者容易理解
  - 机器容易理解结构（搜索、读屏软件）
  - SEO
4. property 和 attribute 的区别
  - attribute 是“死”的
  - property 是“活”的

### 2 CSS 基础

1. 选择器解析从右往左解析，是为了提高解析速度
2. 计算选择器的优先级按照下面的顺序：  
  + !important 声明的样式优先级最高
  + 内联样式
  + ID选择器
  + 类选择器，属性选择器，伪类选择器
  + 类型选择器和伪元素选择器
  + 通用选择器和层级关系的选择器如子选择器、兄弟选择器不参与计算  
  + 注意:not()伪类选择器也不参与运算，但伪类中的选择器会参与运算。
3. content-area 的区域就是 `background` 属性应用的区域。virtual-area 的高度就是
`line-height`，参与计算 line-box 的高度
4. 其他的内联元素：
  - 替换内联元素
  - `inline-block` 和其他 `inline-*`元素
  - 参与特定格式化内容的内联元素（例如 flexbox 中，所有 flex 元素都是 blocksified）   
对于这些特定的内联元素，高度是根据 `height, margin, border` 这些属性计算出的。如果
`height` 是 `auto` 那就是用 `line-height`。
5. 空的行内元素会生成空的 inline-box，不过这些盒子仍然有 margin, padding, border 以及
一个 line-height，因此会以其好像包含内容的形式影响 line box 高度的计算。
6. 注意 `line-height` 定义在不同元素上的含义，在 inline 元素上指其 inline box 的高度，
在块容器盒上指其包含元素的 line boxes 的最小高度
7. `baseline, sub, super, middle, text-top, text-bottom`, `top, bottom`
8. inline-block 的基线是其常规流内最后的 line box 的基线，除非其不包含 in-flow 的
line box，或者其 overflow 属性不是 visible，这两种情况下其基线是 margin 下边缘
9. `font-weight`, normal = 400, bold = 700, bolder 和 lighter 是不固定的，可能取决
于父元素，100~900 只能为整百
10. 注意 overflow: hidden 剪裁的是 content，因此 padding 超出是不算的。

### 3 布局和效果

1. 布局方案
  - table
  - flex
  - grid
  - float
  - inline-block
2. box-shadow 的扩展半径可以为负值，则可以使用扩展半径做一份元素的副本，然后还可以放大
缩小。
3. 重新思考盒模型，content box, padding box, border box, margin box。这是 4 个独立的
盒子吗？    

```html
<div id="test">
    <div class="left">左边，无高度属性，自适应于最高一栏的高度</div>
    <div class="right">右边，无高度属性，自适应于最高一栏的高度</div>
    <div class="center">中间，高度300像素，左右两栏的高度与之自适应</div>
</div>
```    

```css
#test{overflow:hidden; zoom:1;}
.left{width:200px; margin-bottom:-3000px; padding-bottom:3000px; background:#cad5eb; float:left;}
.right{width:400px; margin-bottom:-3000px; padding-bottom:3000px; background:#f0f3f9; float:right;}
.center{height:300px; margin:0 410px 0 210px; background:#ffe6b8;}
```    

为什么会这样？从这个例子能很明显的看出，margin box 和 content box 是独立的，而且 BFC
在布局规则上依赖于 margin box。   

在视觉格式化模型那一部分对 BFC 中这样说：在一个 BFC 中，盒子垂直方向上一个挨一个，从包含块
的顶部开始，两个兄弟盒子之间的垂直距离，由 margin 属性决定。也就是说布局上是依据 margin
box 的，而不是 content box, padding box, border box。   

### 3 html 中的空白处理

1. IFC 中的处理：   

```
<h1>。。。Hello。◁
→→→→<span>。World!</span>→。。</h1>
```    

处理步骤如下：   

+ 首先，换行符前后的所有空格和 tab 都被删除掉。这时变为：   

```
<h1>。。。Hello◁
<span>。World!</span>→。。</h1>
```   

+ 之后，将所有 tab 替换为空格，换行替换为空格 `<h1>。。。Hello。<span>。World!</span>。。。</h1>`    
+ 多个相连的空格替换为 1 个 `<h1>。Hello。<span>World!</span>。</h1>`
+ 行首和追尾的空格删除。 `<h1>Hello<span>World!</span></h1>`   

