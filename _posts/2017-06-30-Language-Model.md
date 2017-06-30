
# 语言模型 Language Model

* 语言模型是一串字符串组成一句话的概率
* 语言模型中的条件概率时给定n个词来估计下一个位置n+1时的词的概率



## 语言模型简介	

**语言模型计算** 一个长度为 m 的字符串表示一句话，那么可以利用下式一句话的概率：
<a href="http://www.codecogs.com/eqnedit.php?latex=\small&space;P(w_1,w_2,...,w_m)&space;=&space;P(w_1)P(w_2|w_1)P(w_3|w_1,w_2)...P(w_m|w_1,w_2,...,w_{m-1})" target="_blank"><img src="http://latex.codecogs.com/gif.latex?\small&space;P(w_1,w_2,...,w_m)&space;=&space;P(w_1)P(w_2|w_1)P(w_3|w_1,w_2)...P(w_m|w_1,w_2,...,w_{m-1})" title="\small P(w_1,w_2,...,w_m) = P(w_1)P(w_2|w_1)P(w_3|w_1,w_2)...P(w_m|w_1,w_2,...,w_{m-1})" /></a>

**N-gram模型** 在计算条件概率时距离大于 n 的上下文的词会被忽略，对**语言模型计算**中的条件概率计算做了以下近似：

<img src="http://latex.codecogs.com/gif.latex?\small&space;P(w_m|w_1,w_2,...,w_{m-1})\approx&space;P(w_m|w_{m-(n-1)},...,w_{m-1})" title="\small P(w_m|w_1,w_2,...,w_{m-1})\approx P(w_m|w_{m-(n-1)},...,w_{m-1})" /></a>

一般来说 n<4,当n=1时称为 unigram model，当 n = 2，3时称为 bigram model、trigram model。这种**N-gram模型**可以保留一定的局部词序信息且可以缓减**语言模型计算**中过长的句子序列长度导致的条件概率计算困难。

下面是**N-gram模型**具体计算条件概率的公式，通过语料库中的频率统计来估计当前的 n-gram 条件概率：

<a href="http://www.codecogs.com/eqnedit.php?latex=\small&space;P(w_m|w_1,w_2,...,w_{m-1})&space;=&space;\frac{count(w_1,w_2,...,w_{m-1},w_m)}{count(w_1,w_2,...,w_{m-1})}" target="_blank"><img src="http://latex.codecogs.com/gif.latex?\small&space;P(w_m|w_1,w_2,...,w_{m-1})&space;=&space;\frac{count(w_1,w_2,...,w_{m-1},w_m)}{count(w_1,w_2,...,w_{m-1})}" title="\small P(w_m|w_1,w_2,...,w_{m-1}) = \frac{count(w_1,w_2,...,w_{m-1},w_m)}{count(w_1,w_2,...,w_{m-1})}" /></a>

其中count(sequence)函数是在语料库中统计当前sequence出现的次数。
## NNLM
## RNNLM
## C&W Model
Collobert和Weston提出的第一个直接生成词向量的模型。**C&W Model**是一种四层神经网络，包括input layer、embedding layer、hidden layer、output layer。其核心思想是对n-gram进行评分：语料库中正常的短语得分高于语料库中未出现的异常短语。网络结构如下所示：
![Markdown](/Users/busishu/Desktop/cw_model.png)
对于整个语料库而言，**C&W Model** 的最小化目标是：
<img src="http://latex.codecogs.com/gif.latex?\small&space;\sum_{(w,c)\in&space;D}&space;\sum_{w'&space;\in&space;V}&space;max(0,1-score(w,c)&plus;score(w',&space;c))" title="\small \sum_{(w,c)\in D} \sum_{w' \in V} max(0,1-score(w,c)+score(w', c))" /></a>

**C&W Model** 的目标词是在输入层的，而之前的NNLM的目标词都是在输出层，极大简化了hidden layer和output layer的计算(由V*h降低到1*h)


## Continuous Bag-of-Words Model
Mikolov在2013的文章[**Mikolov** CBOW and Skip-gram](#CBOW-and-Skip-gram)中第一次提出了CBOW和Skip-gram模型。最初的目的是为了获得更好的词向量。网络结构如下所示： 
![Markdown](/Users/busishu/Desktop/cbow.png)

该模型优点是直接使用了softmaax对目标词进行预测，目标词是选定的n-gram文本的中心词。**CBOW**使用了上下文向量的平均值(加和值)作为输入。对于整个语料库来说，**CBOW**模型的最大化目标是：
<img src="http://latex.codecogs.com/gif.latex?\small&space;\sum_{(w,c)&space;\in&space;D}&space;log&space;P(w|c)" title="\small \sum_{(w,c) \in D} log P(w|c)" /></a>

# Skip-gram Model
遍历目标词上下文中的每个词都作为上下文的向量来预测目标词，**Skip-gram**模型的最大化目标是：
<img src="http://latex.codecogs.com/gif.latex?\small&space;\sum_{(w,c)&space;\in&space;D}&space;\sum_{w_i&space;\in&space;c}&space;log&space;P(w|w_i)" title="\small \sum_{(w,c) \in D} \sum_{w_i \in c} log P(w|w_i)" /></a>


#### Reference style
Sometimes it looks too messy to include big long urls inline, or you want to keep all your urls together.  

Make [a link][arbitrary_id] `[a link][arbitrary_id]` then on it's own line anywhere else in the file:  
`[arbitrary_id]: http://macdown.uranusjr.com "Title"`
  
If the link text itself would make a good id, you can link [like this][] `[like this][]`, then on it's own line anywhere else in the file:  
`[like this]: http://macdown.uranusjr.com`  

[arbitrary_id]: http://macdown.uranusjr.com "Title"
[like this]: http://macdown.uranusjr.com  


### Images
#### Inline
`![Alt Image Text](path/or/url/to.jpg "Optional Title")`
#### Reference style
`![Alt Image Text][image-id]`  
on it's own line elsewhere:  
`[image-id]: path/or/url/to.jpg "Optional Title"`


### Lists

* Lists must be preceded by a blank line (or block element)
* Unordered lists start each item with a `*`
- `-` works too
	* Indent a level to make a nested list
		1. Ordered lists are supported.
		2. Start each item (number-period-space) like `1. `
		42. It doesn't matter what number you use, I will render them sequentially
		1. So you might want to start each line with `1.` and let me sort it out

Here is the code:

```
* Lists must be preceded by a blank line (or block element)
* Unordered lists start each item with a `*`
- `-` works too
	* Indent a level to make a nested list
		1. Ordered lists are supported.
		2. Start each item (number-period-space) like `1. `
		42. It doesn't matter what number you use, I will render them sequentially
		1. So you might want to start each line with `1.` and let me sort it out
```



### Block Quote

> Angle brackets `>` are used for block quotes.  
Technically not every line needs to start with a `>` as long as
there are no empty lines between paragraphs.  
> Looks kinda ugly though.
> > Block quotes can be nested.  
> > > Multiple Levels
>
> Most markdown syntaxes work inside block quotes.
>
> * Lists
> * [Links][arbitrary_id]
> * Etc.

Here is the code:

```
> Angle brackets `>` are used for block quotes.  
Technically not every line needs to start with a `>` as long as
there are no empty lines between paragraphs.  
> Looks kinda ugly though.
> > Block quotes can be nested.  
> > > Multiple Levels
>
> Most markdown syntaxes work inside block quotes.
>
> * Lists
> * [Links][arbitrary_id]
> * Etc.
```
  
  
### Inline Code
`Inline code` is indicated by surrounding it with backticks:  
`` `Inline code` ``

If your ``code has `backticks` `` that need to be displayed, you can use double backticks:  
```` ``Code with `backticks` `` ````  (mind the spaces preceding the final set of backticks)


### Block Code
If you indent at least four spaces or one tab, I'll display a code block.

	print('This is a code block')
	print('The block must be preceded by a blank line')
	print('Then indent at least 4 spaces or 1 tab')
		print('Nesting does nothing. Your code is displayed Literally')

I also know how to do something called [Fenced Code Blocks](#fenced-code-block) which I will tell you about later.

### Horizontal Rules
If you type three asterisks `***` or three dashes `---` on a line, I'll display a horizontal rule:

***


## <a name="markdown-pane"></a>The Markdown Preference Pane
This is where I keep all preferences related to how I parse markdown into html.  
![Markdown preferences pane](http://d.pr/i/RQEi+)

### Document Formatting
The ***Smartypants*** extension automatically transforms straight quotes (`"` and `'`) in your text into typographer’s quotes (`“`, `”`, `‘`, and `’`) according to the context. Very useful if you’re a typography freak like I am. Quote and Smartypants are syntactically incompatible. If both are enabled, Quote takes precedence.


### Block Formatting

#### Table

This is a table:

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

You can align cell contents with syntax like this:

| Left Aligned  | Center Aligned  | Right Aligned |
|:------------- |:---------------:| -------------:|
| col 3 is      | some wordy text |         $1600 |
| col 2 is      | centered        |           $12 |
| zebra stripes | are neat        |            $1 |

The left- and right-most pipes (`|`) are only aesthetic, and can be omitted. The spaces don’t matter, either. Alignment depends solely on `:` marks.

#### <a name="fenced-code-block">Fenced Code Block</a>

This is a fenced code block:

```
print('Hello world!')
```

You can also use waves (`~`) instead of back ticks (`` ` ``):

~~~
print('Hello world!')
~~~


You can add an optional language ID at the end of the first line. The language ID will only be used to highlight the code inside if you tick the ***Enable highlighting in code blocks*** option. This is what happens if you enable it:

![Syntax highlighting example](http://d.pr/i/9HM6+)

I support many popular languages as well as some generic syntax descriptions that can be used if your language of choice is not supported. See [relevant sections on the official site](http://macdown.uranusjr.com/features/) for a full list of supported syntaxes.


### Inline Formatting

The following is a list of optional inline markups supported:

Option name         | Markup           | Result if enabled     |
--------------------|------------------|-----------------------|
Intra-word emphasis | So A\*maz\*ing   | So A<em>maz</em>ing   |
Strikethrough       | \~~Much wow\~~   | <del>Much wow</del>   |
Underline [^under]  | \_So doge\_      | <u>So doge</u>        |
Quote [^quote]      | \"Such editor\"  | <q>Such editor</q>    |
Highlight           | \==So good\==    | <mark>So good</mark>  |
Superscript         | hoge\^(fuga)     | hoge<sup>fuga</sup>   |
Autolink            | http://t.co      | <http://t.co>         |
Footnotes           | [\^4] and [\^4]: | [^4] and footnote 4   |

[^4]: You don't have to use a number. Arbitrary things like `[^footy note4]` and `[^footy note4]:` will also work. But they will *render* as numbered footnotes. Also, no need to keep your footnotes in order, I will sort out the order for you so they appear in the same order they were referenced in the text body. You can even keep some footnotes near where you referenced them, and collect others at the bottom of the file in the traditional place for footnotes. 




## <a name="rendering-pane"></a>The Rendering Preference Pane
This is where I keep preferences relating to how I render and style the parsed markdown in the preview window.  
![Rendering preferences pane](http://d.pr/i/rT4d+)

### CSS
You can choose different css files for me to use to render your html. You can even customize or add your own custom css files.

### Syntax Highlighting
You have already seen how I can syntax highlight your fenced code blocks. See the [Fenced Code Block](#fenced-code-block) section if you haven’t! You can also choose different themes for syntax highlighting.

### TeX-like Math Syntax
I can also render TeX-like math syntaxes, if you allow me to.[^math] I can do inline math like this: \\( 1 + 1 \\) or this (in MathML): <math><mn>1</mn><mo>+</mo><mn>1</mn></math>, and block math:

\\[
    A^T_S = B
\\]

or (in MathML)

<math display="block">
    <msubsup><mi>A</mi> <mi>S</mi> <mi>T</mi></msubsup>
    <mo>=</mo>
    <mi>B</mi>
</math>



### Task List Syntax
1. [x] I can render checkbox list syntax
	* [x] I support nesting
	* [x] I support ordered *and* unordered lists
2. [ ] I don't support clicking checkboxes directly in the html window


### Jekyll front-matter
If you like, I can display Jekyll front-matter in a nice table. Just make sure you put the front-matter at the very beginning of the file, and fence it with `---`. For example:

```
---
title: "Macdown is my friend"
date: 2014-06-06 20:00:00
---
```

#References
<a name="CBOW-and-Skip-gram"></a> [1] TomasMikolov,KaiChen,GregCorrado,andJeffreyDean.Efficientestimation of word representations in vector space. International Conference on Learning Representations Workshop Track, 2013.


