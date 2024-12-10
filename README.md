
本篇开始介绍`Manim`中的**动画**模块，**动画**模块是整个框架的核心魅力所在。


Manim不仅提供了可以直接实现各种各样动画效果的对象，


还提供了设置动画的时长、延迟时间以及运动速率等参数，可以据此发挥自己的创意，自定义出与众不同的动画效果。


本篇主要介绍与文字相关的几个内置的动画效果。


1. `AddTextLetterByLetter`：以逐个字母添加文本的方式来展示文字内容
2. `RemoveTextLetterByLetter`：类似橡皮擦式的文本删除效果
3. `Write`：模拟手写的效果
4. `Unwrite`：与 `Write` 动画相反，用于模拟擦除手写内容或者撤销绘制的过程


# 1\. 动画概述


## 1\.1\. AddTextLetterByLetter


`AddTextLetterByLetter`动画的特点是以逐个字母添加文本的方式来展示文字内容，呈现出一种文字逐步生成的效果。


呈现的过程中可以控制字母出现的速度，让动画的节奏更符合内容需要。


它主要适用于教学视频、讲解类动画等场景。


例如在制作数学定理讲解视频时，逐步展示定理内容，让观众能够逐字跟上节奏，增强理解。


它的主要参数有：




| **参数名称** | **类型** | **说明** |
| --- | --- | --- |
| text | Text | 要逐个显示字母的文本内容 |
| time\_per\_char | float | 字母出现的频率，用于控制每个字母出现的时间间隔 |
| rate\_func | func | 用于控制字母出现的速率函数 |
| run\_time | float | 动画的运行时间 |


## 1\.2\. RemoveTextLetterByLetter


`RemoveTextLetterByLetter`实现文本从后往前逐个字母消失的效果，


和 `AddTextLetterByLetter` 相反，有一种反向的动态感。


它能够和其他动画效果配合，例如在文字逐个删除后，紧接着出现新的文本，形成连贯的内容更新动画。


`RemoveTextLetterByLetter`一般用于橡皮擦式的文本删除效果，在需要撤销输入或者擦除内容的场景下使用。


比如在展示代码编辑过程中，对错误代码进行逐个字母删除的动画。


它的主要参数有：




| **参数名称** | **类型** | **说明** |
| --- | --- | --- |
| text | Text | 要逐个删除字母的文本内容 |
| time\_per\_char | float | 控制每个字母删除的时间间隔，即字母逐个消失的频率 |
| rate\_func | func | 用于控制字母删除的速率函数 |
| run\_time | float | 动画的运行时间 |


## 1\.3\. Write


`Write`动画从对象的起始点开始，以一种类似手写或者绘制的方式来展示对象的出现，给人一种自然生成的感觉。


不仅是文字，对于复杂的图形，也可以根据图形的结构和路径进行书写式的动画展示，而不是简单的整体出现。


因为其模拟手写的效果，`Write`非常适合在数学推导、绘图步骤或者艺术创作过程的展示中使用。


例如，在展示几何图形的绘制步骤时，也可以用 `Write` 动画来模拟手动画图的过程。


它的主要参数有：




| **参数名称** | **类型** | **说明** |
| --- | --- | --- |
| vmobject | VMobject | 要进行手写动画的对象 |
| rate\_func | func | 用于控制书写的速率函数 |
| reverse | bool | 用于控制书写方向是否反向 |


## 1\.4\. Unwrite


`Unwrite`动画与 `Write` 动画相反，用于模拟擦除手写内容或者撤销绘制的过程。


它以一种类似于逆向书写的方式来使对象消失，和 `Write` 动画形成互补的效果。


在教学视频中，如果需要重新讲解某个步骤，可以用 `Unwrite` 动画来清除之前的内容。


它的主要参数有：




| **参数名称** | **类型** | **说明** |
| --- | --- | --- |
| vmobject | VMobject | 要进行擦除的对象 |
| rate\_func | func | 用于控制擦除的速率函数 |
| reverse | bool | 用于控制擦除的顺序（从前往后擦，还是从后往前擦） |


# 2\. 使用示例


下面还是结合一些根据实际场景简化的示例来演示文字创建和销毁相关动画的使用。


## 2\.1\. 模拟知识讲解的视频


在这个模拟知识讲解视频的示例中，先通过`AddTextLetterByLetter`引入问题，引起观众的思考。


然后用`Write`动画展示答案推导过程，帮助观众理解。


之后使用`RemoveTextLetterByLetter`删除问题，避免画面过于杂乱，


最后用`Unwrite`擦除答案，为下一个知识点的讲解做准备。



```
# 首先使用 AddTextLetterByLetter 逐个字母显示问题
question = Text("什么是勾股定理?")
question.shift(UP * 2)
self.play(AddTextLetterByLetter(question))
self.wait()

# 接着使用 Write 动画来展示答案的逐步推导过程
answer = MathTex(r"a^2 + b^2 = c^2", font_size=40)
answer.next_to(question, DOWN)
self.play(Write(answer), run_time=run_time)
self.wait()

# 然后使用 RemoveTextLetterByLetter 逐个字母删除问题
self.play(RemoveTextLetterByLetter(question))
self.wait()

# 最后使用 Unwrite 动画擦除答案
self.play(Unwrite(answer))

```

![](https://img2024.cnblogs.com/blog/83005/202412/83005-20241209132000980-446992189.gif)


## 2\.2\. 模拟故事创作动画


此示例应用于故事创作动画中，`AddTextLetterByLetter`让故事标题逐个字母出现，增加神秘感。


`Write`动画呈现故事开头，使观众沉浸在故事氛围中。


随后`RemoveTextLetterByLetter`和`Unwrite`分别删除故事开头和标题，象征着故事一个段落的结束，为后续情节发展腾出画面空间。


其中`RemoveTextLetterByLetter`设置了`reverse`参数为`False`，这样删除字母的顺序变成了从头到尾。



```
# 用 AddTextLetterByLetter 显示故事标题
title = Text("The Mysterious Forest", color=YELLOW)
title.shift(UP * 2)
self.play(AddTextLetterByLetter(title))
self.wait()

# 使用 Write 动画展示故事的开头描述
story = Text(
    "Once upon a time, \nthere was a young adventurer \nwho entered the forest.",
    font_size=30,
)
story.next_to(title, DOWN)
self.play(Write(story))
self.wait()

# 用 RemoveTextLetterByLetter 逐个字母删除故事开头
self.play(RemoveTextLetterByLetter(story))
self.wait()

# 使用 Unwrite 动画擦除标题
self.play(Unwrite(title, reverse=False))

```

![](https://img2024.cnblogs.com/blog/83005/202412/83005-20241209132000924-553569261.gif)


## 2\.3\. 不用速率显示文本


在这个示例中，主要演示`rate_func`参数的使用。


分别使用3种不同的速率来显示文本，第一行文本的显示速率是时间的平方根，所以会逐渐变慢；


第二行文本的显示速率是线性的，所以文本逐个匀速显示出来；


第二行文本的显示速率是时间的平方，所以显示速度越来越快。


这样就展示了在`manim`中如何利用`rate_func`参数来实现不同速率的文本显示动画效果。



```
# 准备要显示的文本
txt1 = Text("Slow speed for display text", font_size=30, color=BLUE)
txt2 = Text("Normal speed for display text", font_size=30, color=RED)
txt3 = Text("Fast speed for display text", font_size=30, color=GREEN)

txt1.shift(UP * 2)
# 设置不同的 rate_func 来控制文本出现速率
# 越来越慢的速率，t 的平方根函数
self.play(AddTextLetterByLetter(txt1, rate_func=lambda t: t**0.5))
self.wait()

txt2.next_to(txt1, DOWN)
# 使用线性速率函数快速显示文本
self.play(AddTextLetterByLetter(txt2, rate_func=linear))
self.wait()

txt3.next_to(txt2, DOWN)
# 越来越快的速率，t 的平方函数
self.play(AddTextLetterByLetter(txt3, rate_func=lambda t: t**2))
self.wait()

# 清除场景中的文本
self.play(Unwrite(txt1), Unwrite(txt2), Unwrite(txt3))


```

![](https://img2024.cnblogs.com/blog/83005/202412/83005-20241209132001155-1065272867.gif)


# 3\. 附件


文中的代码只是关键部分的截取，完整的代码共享在网盘中（`text.py`），


下载地址: [完整代码](https://github.com) (访问密码: 6872\)


 本博客参考[milou加速器](https://xinminxuehui.org)。转载请注明出处！
