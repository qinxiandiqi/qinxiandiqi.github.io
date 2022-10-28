---
# type: posts 
title: "android的属性动画"
date: 2016-06-20T12:48:17+0800
authors: ["Jianan"]
summary: "属性动画（Property Animation）系统是一个更加强大的框架，它几乎允许你为任何东西设置动画。不管一个对象是否需要绘制到屏幕上面，你都可以定义一个动画让这个对象的属性随着时间推移而改变。一个属性动画可以在规定的时间内改变一个属性值（对象的一个成员变量）。设定动画，你需要指定对象中需要设定动画的属性，例如对象在屏幕上的坐标，动画需要执行的时间，以及动画过程中属性的变化值。"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["android", "属性动画", "动画"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 4221
comment_num: 0
---

> 原文作者：Google
原文地址：[https://developer.android.com/guide/topics/graphics/prop-animation.html](https://developer.android.com/guide/topics/graphics/prop-animation.html)  
原文版权：[Creative Commons 2.5 Attribution License](http://creativecommons.org/licenses/by/2.5/)  
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2016-06-17版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途

<br>
# 前言
---

属性动画（Property Animation）系统是一个更加强大的框架，它几乎允许你为任何东西设置动画。不管一个对象是否需要绘制到屏幕上面，你都可以定义一个动画让这个对象的属性随着时间推移而改变。一个属性动画可以在规定的时间内改变一个属性值（对象的一个成员变量）。设定动画，你需要指定对象中需要设定动画的属性，例如对象在屏幕上的坐标，动画需要执行的时间，以及动画过程中属性的变化值。  

属性动画系统允许你为一个动画设定以下属性：

* 时长：你可以指定动画的时长。默认的动画时长是300毫秒。
* 时间插值器：你可以设定一个根据当前动画已经执行的时间计算出对应属性值的方法（它被称为时间插值器）。
* 重复次数和行为：你可以设定当动画结束之后是否需要自动重复执行，以及可以重复执行多少次。你同样也可以设定动画是否需要反向回放，设定动画来回反复反向回放，直到完成动画设定的重复次数。
* 动画组：你可以在一个动画组里面逻辑嵌套多个动画，让这些动画同时播放，或者顺序播放，或者延迟一定的时间后播放。
* 延迟帧刷新：你可以设定动画帧与帧之间的刷新频率。默认设定是每10毫秒刷新一次，但你的应用程序实际刷新的帧频率最终还是要取决于整体系统的繁忙程度，以及系统底层能够支持的定时器有多快。

<br>
# 属性动画如何运作
---

首先，我们通过一个简单的示例来了解下动画的工作过程。图1假设了一个对象需要对它的x属性设定动画，这个x属性代表这个对象在屏幕上面的横坐标。这个动画的时间设定为40毫秒，以及需要移动的距离是40个像素。每过10毫秒（默认的帧频率），这个对象就会在水平方向上移动10个像素。等到40毫秒之后，这个动画停止，对象在水平方向上总共移动了40个像素。这个例子中的动画使用了一个线性插值器，意味着这个对象以匀速移动。  

![animation-linear.png](1005e540c862f3b510c9d2fb34ce8fae.png)

图1：线性动画示例  

你也可以设定动画使用一个非线性插值器。图2演示了一个假设的对象在动画的一开始进行加速，等动画快结束的时候进行减速。这个对象一样在40毫秒中总共移动40个像素点，但移动是非线性的。在一开始，动画加速移动到中间点，然后从中间点一直减速到动画结束。正如图2所示，一开始和将近结束的时候移动的距离比中间时段移动的距离要小。  

![animation-nonlinear.png](c44323fba92aa1b97b288360cf40eae7.png)

图2：非线性动画示例

让我们来仔细看一下属性动画系统的重要组成部分是如何像上面的插图一样计算动画的。图3描述了属性动画系统中一些主要类之间的相互关系。

![valueanimator.png](81fc56819cdb77aae83df27e82e7cbdf.png)

图3：动画如何计算

**ValueAnimator**对象负责跟踪动画的时序，比如这个动画已经运行了多久，以及当前的动画属性值是多少。  

**ValueAnimator**对象中包含了一个**TimeInterpolator**对象，用来定义动画的插值器；另外还包含了一个**TypeEvaluator**对象，这个对象定义了如何根据动画的执行程度计算出对应的属性值。例如，在图2中，**TimeInterpolator**使用的是**AccelerateDecelerateInterpolator**，**TypeEvaluator**使用的是**IntEvaluator**。  

启动一个动画，需要创建一个ValueAnimator对象，并给它设定动画开始和结束时的属性值，以及动画的执行时间。当你调用**start()**方法启动动画，在整个动画执行期间，ValueAnimator会基于整个动画需要执行的时间以及当前已经执行的动画时间，计算出一个从0到1的值，表示已消耗的时间比例。这个已消耗的时间比例代表了动画已完成的百分比，0表示0%，1就表示100%。例如，在图1中，当t=10ms的时候，这个已消耗的时间比例就是0.25，因为整个动画时长为40ms。  

当ValueAnimator完成了一次已消耗的时间比例计算，它就会调用当前设定的TimeInterpolator去计算对应的（属性）插值比例。插值比例根据所设定的时间插值器将已消耗的时间比例映射到一个新的比例值上。例如，在图2中，由于动画缓慢加速，在t=10ms的时候，它的插值比例大概是0.15，显然比已消耗的时间比例0.25要小，因此在这个时间点上，属性值将会是0.15 * （40-0），也就是6个像素点。  

在[API Demo](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/animation/index.html)的**com.example.android.apis.animation** 包中提供了一些关于如何使用属性动画系统的实例代码。

<br>
# 属性动画与视图动画的区别
---

视图动画（View Animation）系统只提供了给View对象设置动画的功能，如果你要给非View对象设置动画，你只能完全依靠自己来实现相关的逻辑代码。实际上，视图动画还受限于只能对View对象的一部分属性设置动画，例如它可以缩放和旋转一个View，但是无法改变view的背景颜色等等。  

视图动画系统还有一个缺点就是它实际上只修改了View对象的绘制位置，而不是View的本身。举个例子，使用视图动画你可以将一个按钮从屏幕上移动过去，这个按钮能够正确的绘制到目标位置上，但这个按钮的点击位置实际上没有随着按钮移动而移动，因此你需要自己实现对应的代码响应逻辑来处理这个问题。  

使用属性动画系统就完全没有这样的限制，你可以给一个对象（View对象或者非View对象）的任意属性设定动画，随着动画的执行，对象的属性也会跟着一起改变。属性动画系统同样提供了更强大的功能来实现动画协作。在更高的等级上，你可以为多个属性添加多个动画（例如颜色、坐标、大小），然后定义动画的各个方面（例如插值器和多个动画同步运作）。  

视图动画系统，相对的，配置起来需要花费的时间会少一点，需要的代码量也会少一点。如果视图动画已经能够满足你所有的要求，或者你现有的代码中已经使用了视图动画，那么你也不是非得使用属性动画系统才可以。在某些特别的情况下，为不同的情景使用不同的动画系统也是合情合理的。  

<br>
# API概览
---

你可以在**android.animation** 包中找到大部分属性动画系统的API。由于视图动画系统已经在**android.view.animation** 包中定义了很多插值器，你也可以在属性动画系统中直接使用这些插值器。下面的表格中列举了属性动画系统的主要组件。  

**Animator** 类中提供了创建动画的基本结构。一般情况下你不需要直接使用这个类，继承这个类只能够提供满足属性动画的最少方法。一般使用的是下面这些Animator的子类：  

表1：Animator
|类名|描述|
|----|----|
|ValueAnimator|属性动画主要的时序引擎，它还要计算动画过程中的属性值。它包含了全部计算动画值的核心功能，还包含了每一个动画的时序细节、关于动画重复的信息、接收更新事件的监听器、以及设定自定义类型来计算的功能。执行属性动画包含两个部分：计算变化的属性值以及给对象设定计算出来的属性值，属性动画就是这么执行的。ValueAnimator不负责第二部分，因此你需要监听ValueAnimator计算出来的属性值更新，并使用自己的代码逻辑把这些属性值赋值给你想要执行动画的属性。更多细节请参考下面的**使用ValueAnimator执行动画**章节。|
|ObjectAnimator|这是ValueAnimator的一个子类，它允许你设定一个目标对象和目标对象中需要执行动画的属性。这个类会在计算出新的属性值时自动更新对象的属性。大多数情况下，你都会使用ObjectAnimator，因为这个类能让设定目标对象的属性动画更加容易。然而，也有一些情况下需要直接使用ValueAnimator，因为ObjectAnimator有一些限制，例如它需要在目标对象上指定特定的计算方法。|
|AnimatorSet|这个类提供了一个机制来组合多个动画，让动画与动画之间关联起来。你可以设定多个动画同时执行，或者顺序执行，或者延迟一段时间后执行。更多细节请参考下面**使用AnimatorSet编排多个动画**章节。|  
<br>
计算器（Evaluator）用于告知属性动画系统如何计算属性值。它们持有Animator类提供的时序数据，动画的初始值和结束值。动画执行过程中的属性值都依赖于这些时序数据。属性动画系统提供了下列计算器：  

表2：计算器
|类名/接口|描述|
|----|----|
|IntEvaluator|默认的用于计算int类型属性的计算器。|
|FloatEvaluator|默认的用于计算float类型属性的计算器。|
|ArgbEvaluator|默认的用于计算16进制颜色值属性的计算器。|
|TypeEvaluator|用于创建自定义计算器的接口。如果你需要设定动画的属性类型不是int、float或者颜色值，那么你就必须实现这个TypeEvaluator接口来定义如何计算动画执行期间的属性值。如果你想要改变int、float或者颜色值类型计算器的默认计算行为，你也可以自定义一个TypeEvaluator实现类来计算这些类型。参考下面**使用TypeEvaluator**章节来获取关于如何自定义计算器的内容。|
<br>
时间插值器中定义了一个动画执行过程中针对指定值的时间函数。例如，你可以设定动画在整个过程中线性变化，也就是在整个动画过程中动画会一直均匀的移动。同样的，你也可以设定动画使用非线性的时间过渡，例如，动画在一开始的时候加速，在结尾的时候减速。表3中列举了**android.view.animation**包中的插值器。如果没有找到适合你的插值器，你可以实现**TimeInterpolator**接口自定义一个插值器。参考下面**使用插值器**章节获取更多如何自动以插值器的内容。

表3：插值器
|类/接口|描述|
|----|----|
|AccelerateDecelerateInterpolator|这个插值器的速率在动画开始和结束的时候变慢，但是在动画的中间会加速。|
|AccelerateInterpolator|这个插值器的速率会在动画开始的时候缓慢，然后慢慢加速。|
|AnticipateInterpolator|这个插值器会在动画开始的时候向反方向运动，然后再甩回前进方向。|
|AnticipateOvershootInterpolator|这个插值器会在动画开始的时候向反方向运动再甩回前进方向，达到动画结束值之后会继续超过结束值再降回结束值。|
|BounceInterpolator|这个插值器会在动画结束的时候反弹跳动。|
|CycleInterpolator|这个插值器会让动画按照指定的次数反复执行。|
|DeceleraterInterpolator|这个插值器的速率会在动画开始的时候很快，然后不停减速。|
|LinearInterpolator|这个插值器的速率会保持匀速。|
|OvershootInterpolator|这个插值器会让动画在一开始向前甩出去直到超出设定的结束值后再返回到结束值。|
|TimeInterpolator|实现自定义的插值器的接口。|

<br>
# 使用ValueAnimator设定动画
---

ValueAnimator类让你通过设定一些类型值，比如一组int、float或者颜色值，在动画执行期间进行动画过渡。你可以调用ValueAnimator的任意一个工厂方法来获取一个ValueAnimator实例：ofInt()，ofFloat()，或者ofObject()。例如：

```java
ValueAnimator animation = ValueAnimator.ofFloat(0f, 1f);
animation.setDuration(1000);
animation.start();
```

在这段代码中，ValueAnimator调用start()方法后，开始在1000毫秒的时间里面从0到1计算动画过程中对应的数值。

你也可以使用下面的代码指定一个自定义的动画值类型：

```java
ValueAnimator animation = ValueAnimator.ofObject(new MyTypeEvaluator(), startPropertyValue, endPropertyValue);
animation.setDuration(1000);
animation.start();
```

在这段代码中，ValueAnimator调用start()方法之后，开始使用MyTypeEvaluator提供的计算逻辑，在1000毫秒里面从startPropertyValue到endPropertyValue计算动画过程中对应的数值。

在上面的代码片段中，其实，并不会对任何对象产生影响，因为ValueAnimator不负责直接操作对象或者属性。你可能想要做的事是根据这些计算值修改需要设定动画的对象。为了实现这个功能，你需要实现一个ValueAnimator中的listener，通过listener在动画的生命周期中适当处理一些重要的事件，例如帧更新。当实现这些listener的时候，你可以通过**getAnimatedValue()** 方法获取特定帧刷新时的动画值。更多详细内容，请参考下面的**动画监听器**章节。

<br>
# 使用ObjectAnimator设定动画
---

ObjectAnimator是ValueAnimator的一个子类（前面章节已经提过了），它集成了时序引擎和ValueAnimator的计算值能力，能够直接操作目标对象中指定的属性。这就让设定任意对象的动画变得更加容易，因为你不再需要去实现ValueAnimator.AnimatorUpdateListener接口，动画属性会自己自动更新。  

实例化一个ObjectAnimator对象跟ValueAnimator很类似，但除了设定动画起始值之外，还需要指定一个对象，并以字符串的形式传入这个对象中需要设定动画的属性名称：

```java
ObjectAnimator anim = ObjectAnimator.ofFloat(foo, "alpha", 0f, 1f);
anim.setDuration(1000);
anim.start();
```

为了让ObjectAnimator能够正确更新属性，你必须按照下面步骤操作：

* 需要设定动画的对象属性必须按照**set&lt;PropertyName>()** 的形式生成它的setter方法（驼峰风格）。因为ObjectAnimator要在动画执行过程中自动更新属性，它就必须能够通过setter方法访问到这个属性。举个例子，如果属性名是“foo”，你就必须有一个setFoo()方法。如果setter方法真的不存在，你可以有三种选择：
    * 如果你有修改这个类的权限，你可以给这个类添加上对应的setter方法。
    * 使用一个你有权限控制的包装类，在类中使用一个有效的setter方法接收包装值后发送给原对象。
    * 使用ValueAnimator代替  
* 如果你在ObjectAnimator的工厂方法中给values...参数只设定了一个值，那么这个值会作为动画的结束值使用。因此，你要设定动画的属性还必须提供一个getter方法，用来获取动画开始的属性初始值。这个getter方法必须按照**get&lt;PropertyName>()** 格式书写。例如，如果这个属性名是“foo”，那么他的getter方法就应该是getFoo()。    
* 需要设定动画的属性getter（如果需要的话）和setter方法操作的数据类型应该与你在ObjectAnimator中指定的数据类型保持一致。例如，如果你使用下面的方法来构造一个ObjectAnimator对象，你就必须持有targetObject.setPropName(float)和targetObject.getPropName(float)方法：
```java
ObjectAnimator.ofFloat(targetObject, "propName", 1f)
```
* 取决于你设定动画的属性或者对象，你可能需要调用View的**invalidate()** 方法来强制屏幕重新绘制属性值有更新的目标对象。你可以在onAnimationUpdate()回调方法中执行这些操作。例如，要执行一个Drawable对象的颜色属性动画，它只会在屏幕重新绘制这个对象后才能看到颜色动画效果。View对象每一个属性的setter方法，例如setAlpha()和setTranslationX()方法都会自动调用invalidate()来刷新View的属性，因此，当调用这些方法给View设定新的属性值时，不需要调用invalidate()方法。更多关于监听器的细节，请参考下面**动画监听器**章节。

<br>
# 使用AnimatorSet编排多个动画
---

在一些情况下，你需要在一个动画启动或者结束的同时启动另一个动画。android系统允许你使用一个AnimatorSet捆绑多个动画，这样你就可以设定多个动画是否同时执行，还是顺序或者延迟一段时间后再执行。你甚至可以在AnimatorSet对象中嵌套另一个AnimatorSet对象。  

下面的示例代码取自[Bouncing Balls](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/animation/BouncingBalls.html)示例项目（为修改方便起见），它按照下面的顺序执行动画：
1. 执行bounceAnim动画。
2. 同时执行squashAnim1、squashAnim2、stretchAnim1和stretchAnim2四个动画。
3. 执行bounceBackAnim动画。
4. 执行fadeAnim动画。

```java
AnimatorSet bouncer = new AnimatorSet();
bouncer.play(bounceAnim).before(squashAnim1);
bouncer.play(squashAnim1).with(squashAnim2);
bouncer.play(squashAnim1).with(stretchAnim1);
bouncer.play(squashAnim1).with(stretchAnim2);
bouncer.play(bounceBackAnim).after(stretchAnim2);
ValueAnimator fadeAnim = ObjectAnimator.ofFloat(newBall, "alpha", 1f, 0f);
fadeAnim.setDuration(250);
AnimatorSet animatorSet = new AnimatorSet();
animatorSet.play(bouncer).before(fadeAnim);
animatorSet.start();
```

更多如何使用动画套件的复杂示例，请查看[Bouncing Balls](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/animation/BouncingBalls.html)示例项目。

<br>
# 动画监听器
---

你可以使用下面列举的listener（监听器）监听动画执行过程中的重要事件：

* Animator.AnimatorListener
    * onAnimationStart() - 当动画开始执行时回调。
    * onAnimationEnd() - 当动画结束的时候回调。
    * onAnimationRepeat() - 当动画重复的时候回调。
    * onAnimationCancel() - 当动画被取消的时候回调。无论动画是怎么被结束，即使是被取消的动画，同时也会回调onAnimationEnd()方法。
* ValueAnimator.AnimatorUpdateListener
    * onAnimationUpdate() - 动画的每一次帧刷新都会回调。监听这个事件是为了在动画执行过程中使用ValueAnimator计算出来的值。为了使用这些值，需要借用这个回调方法带过来的参数——ValueAnimator实例对象，使用ValueAnimator.getAnimatedValue()方法获取当前动画状态对应的动画值。如果你是直接使用ValueAnimator来实现属性动画，那么你就需要实现这个方法。取决于你设定动画的属性或者对象，你可能需要调用View的**invalidate()** 方法来强制屏幕重新绘制属性值有更新的目标对象。你可以在onAnimationUpdate()回调方法中执行这些操作。例如，要执行一个Drawable对象的颜色属性动画，它只会在屏幕重新绘制这个对象后才能看到颜色动画效果。View对象每一个属性的setter方法，例如setAlpha()和setTranslationX()方法都会自动调用invalidate()来刷新View的属性，因此，当调用这些方法给View设定新的属性值时，不需要调用invalidate()方法。更多关于监听器的细节，请参考下面**动画监听器**章节。  

如果你不想实现Animator.AnimatorListener接口中的每一个方法，那么你可以选择继承AnimatorListenerAdapter类来取代直接实现Animator.AnimatorListener接口。AnimatorListenerAdapter类提供了Animator.AnimatorListener接口中每一个方法的空实现，你可以有选择的重写其中几个。  

例如，在[Bouncing Balls](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/animation/BouncingBalls.html)示例项目中创建了一个仅仅重写onAnimationEnd()回调方法的AnimatorListenerAdapter:

```java
ValueAnimatorAnimator fadeAnim = ObjectAnimator.ofFloat(newBall, "alpha", 1f, 0f);
fadeAnim.setDuration(250);
fadeAnim.addListener(new AnimatorListenerAdapter() {
public void onAnimationEnd(Animator animation) {
    balls.remove(((ObjectAnimator)animation).getTarget());
}
```

<br>
# 为ViewGroup设定布局动画
---

属性动画系统提供了为ViewGroup对象设定布局动画的功能，这些功能使用起来的效果就像直接为布局中的子View设定动画一样。  

你可以使用**LayoutTransition**类来为ViewGroup内部对象的改变过程设定动画。当你在一个ViewGroup中添加或移除一个子View，又或者是使用View.setVisibility()给子view设定VISIBLE、INVISIBLE和GONE值，这个子view变化的过程都可以添加显示和消失动画。当你添加或者移除子view的时候，剩余的子view一样也可以使用动画移动到新的位置上。你可以在LayoutTransition对象的**setAnimator()**方法中设定这些动画，这个方法需要传入一个Animator对象和下面列举的一个LayoutTransition常量作为参数值：

* **APPEARING** - 这个标识表明设定的动画要在子view从视图容器中显示出来的过程中执行。
* **CHANGE_APPEARING** - 这个标识表明设定的动画要在视图容器中原有的子View由于要显示其它子View而引起自身布局变化时执行。
* **DISAPPEARING** - 这个标识表明设定的动画要在子View从视图容器中消失的时候执行。
* **CHANGE_DISAPPEARING** - 这个标识表明设定的动画要在视图容器中的子view由于其它子view的小时而引起自身布局变化的时候执行。

你可以为你的布局过渡定义上面四种事件类型的自定义动画，也可以让动画系统直接使用默认的动画。

[LayoutAnimations](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/animation/LayoutAnimations.html)示例项目中介绍了如何为布局过渡定义动画，以及如何把动画设定到View对象上面。  

[LayoutAnimationsByDefault](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/animation/LayoutAnimationsByDefault.html)示例项目和它的layout_animations_by_default.xml布局文件介绍了如何在xml文件中为ViewGroup开启默认的布局动画。你唯一需要做的事情就是讲ViewGroup的**android:animationLayoutChanges**属性设置为true。例如：
```xml
<LinearLayout
    android:orientation="vertical"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:id="@+id/verticalContainer"
    android:animateLayoutChanges="true" />
```

将这个属性设置为true之后，ViewGroup中的添加或者移除子View都会有动画效果，同时仍然保留在ViewGroup中的View在这个过程中也会有动画过度效果。  

<br>
# 使用TypeEvaluator（值计算器）
---

如果你要给一个android系统未支持的类型添加动画，你可以通过实现TypeEvaluator接口来创建一个自定义计算器。android系统已支持的类型包括int、float、颜色值，它们分别通过IntEvaluator、FloatEvaluator和ArgbEvaluator计算器来实现动画支持。  

TypeEvaluator接口中只包含了一个方法evaluate()。这个方法允许你根据动画当前的执行进度点返回一个合适的属性值。FloatEvaluator实现这个过程的代码如下：

```java
public class FloatEvaluator implements TypeEvaluator {

    public Object evaluate(float fraction, Object startValue, Object endValue) {
        float startFloat = ((Number) startValue).floatValue();
        return startFloat + fraction * (((Number) endValue).floatValue() - startFloat);
    }
}
```

> 注意：当ValueAnimator（或者ObjectAnimator）运行的过程中，它会计算当前动画已消耗的时间比例（一个0到1之间的值），然后再根据你使用的插值器计算出一个插值比例。这个插值比例就是就是你在TypeEvaluator.evaluate()方法中接收到的fraction参数，因此你不需要在计算动画属性值的时候关心如何计算插值。

<br>
# 使用插值器
---

插值器定义了动画在运行过程中属性值的时间函数。例如，你可以让动画在整个执行过程中线性过度，这意味着动画在整段时间里面都做匀速运动。你也可以设定动画非线性过度，例如，在动画开始的时候加速，在动画结束的时候减速。

在动画系统中，插值器会从Animator接收到一个代表从动画开始到目前已花费的时间比例值。插值器会整合动画的类型去修改这个比例值。android在android.view.animation包中提供了一系列通用的插值器。如果你在里面没有找到你想要的插值器，你可以实现TimeInterpolator接口来自定义一个插值器。  

作为例子，下面比较了默认的AccelerateDecelerateInterpolator和LinearInterpolator插值器是如何计算插值比例的。LinearInterpolator插值器没有对已消耗的时间比例进行修改。AccelerateDecelerateInterpolator插值器在开始的时候调整加大已消耗的时间比例，在结束的时候降低已消耗的时间比例。下面的代码列举了这些插值器的逻辑：

**AccelerateDecelerateInterpolator**
```java
public float getInterpolation(float input) {
    return (float)(Math.cos((input + 1) * Math.PI) / 2.0f) + 0.5f;
}
```

**LinearInterpolator**
```java
public float getInterpolation(float input) {
    return input;
}
```

下面的表格列举了一个耗时1000毫秒的动画在执行过程中使用这些插值器计算出来的大概值：

|已消耗的时间（ms）|已消耗的时间比例/线性插值比例|（加速/减速）插值比例|
|----|----|----|
|0|0|0|
|200|0.2|0.1|
|400|0.4|0.345|
|600|0.6|0.8|
|800|0.8|0.9|
|100|1|1|
<br>
正如表格所描述的，LinearInterpolator在每个时间段里面修改的插值都是一样，即每过200毫秒增加0.2。AccelerateDecelerateInterpolator从200毫秒到600毫秒之间修改的插值比LinearInterpolator快，在600毫秒到1000毫秒之间又比LinearInterpolator慢。

<br>
# 指定关键帧
---

**Keyframe**对象中包含了一对时间与属性值的键值对，它定义了一个动画在指定的时间点上要呈现出特定的状态。每一个Keyframe都可以设定自己的插值器，用来控制动画从上一个Keyframe到当前这个Keyframe这段时间间隔里面的行为。

获取一个Keyframe实例对象，你必须选择一个Keyframe的ofInt()、ofFloat()，或者ofObject()工厂方法来构造一个合适的Keyframe对象。然后，再调用**ofKeyframe()**方法生成一个**PropertyValuesHolder**对象。一旦你生成了而这个对象，你就可以将这个PropertyValuesHolder对象以及需要设定动画的对象作为参数生成一个Animator。下面的代码片段演示了这个过程：
```java
Keyframe kf0 = Keyframe.ofFloat(0f, 0f);
Keyframe kf1 = Keyframe.ofFloat(.5f, 360f);
Keyframe kf2 = Keyframe.ofFloat(1f, 0f);
PropertyValuesHolder pvhRotation = PropertyValuesHolder.ofKeyframe("rotation", kf0, kf1, kf2);
ObjectAnimator rotationAnim = ObjectAnimator.ofPropertyValuesHolder(target, pvhRotation)
rotationAnim.setDuration(5000ms);
```

更多关于使用关键帧的复杂示例，请查看[MultiPropertyAnimation](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/animation/MultiPropertyAnimation.html)示例项目。

<br>
# 给View添加动画
---

属性动画系统允许为View对象设定流线型的动画，并且提供了一些比视图动画系统更加高级的功能。视图动画系统是通过修改View的绘制位置来改变View对象。这些操作是在每个View的容器中进行处理的，因为View本身并没有可以执行这类操作的属性。这就会导致View已经开始动画了，但是View对象本身并没有任何变化。整个过程看起来就像是View对象已经被绘制到屏幕上面的另一个位置，但它实际上还是在原来的位置上。从android 3.0开始，View类添加了新的属性以及对应的getter和setter方法，以解决这个缺陷。  

属性动画系统可以通过直接修改View对象的实际属性来完成它在屏幕上的动画效果。另外，当View的属性有变化的时候，它也会自动调用**invalidate()**方法刷新屏幕。View类中支持属性动画的新属性如下：

* **translationX**和**translationY**：这两个属性以布局容器给View的left和top坐标值为基准，控制View在横竖方向上需要移动位置的增量值。  
* **rotation**、**rotationX**和**rotationY**：这三个属性以View的中心点为基准，控制view在2D视觉（rotation属性）和3D视觉上的旋转。  
* **scaleX**和**scaleY**：这两个属性以View中心点为基准，控制View在2D视觉上的大小缩放。
* **pivotX**和**pivotY**：这两个属性控制View对象的中心点，当View进行旋转或者缩放的时候需要依赖这个中心点的位置。默认情况下，中心点的位置在View对象的中心。  
* **x**和**y**：这两个属性描述了View在它所在的视图容器中的最终位置，可以看做是left或者top和translationX或者translationY值的和。  
* **alpha**：代表View的透明度。默认值为1，表示不透明，当值为0时即为完全透明（不显示）。  

给View对象的属性设定动画，例如它的颜色或者旋转值，你需要做的只是创建一个属性的Animator，然后指定需要设定动画的属性。例如：
```java
ObjectAnimator.ofFloat(myView, "rotation", 0f, 360f);
```

更多关于创建动画的细节，请参考上面**使用ValueAnimator设定动画**和**使用ObjectAnimator设定动画**章节。

<br>
# 使用ViewPropertyAnimator设定动画
---

**ViewPropertyAnimator**底层封装了一个Animator对象，提供了一种简单的途径来给View的多个属性值同时设置动画。它的操作与ObjectAnimator很类似，因为它也是直接修改view的属性值，不同的是它能够一次操作多个属性值。此外，使用ViewPropertyAnimator的代码会更加简洁和易读。下面的代码片段比较了同时给view的x和y属性设定动画时，使用多个ObjectAnimator对象、使用一个ObjectAnimator对象，以及使用ViewPropertyAnimator对象之间的区别。

**多个ObjectAnimator对象**
```java
ObjectAnimator animX = ObjectAnimator.ofFloat(myView, "x", 50f);
ObjectAnimator animY = ObjectAnimator.ofFloat(myView, "y", 100f);
AnimatorSet animSetXY = new AnimatorSet();
animSetXY.playTogether(animX, animY);
animSetXY.start();
```

**一个ObjectAnimator对象**
```java
PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat("x", 50f);
PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat("y", 100f);
ObjectAnimator.ofPropertyValuesHolder(myView, pvhX, pvyY).start();
```

**ViewPropertyAnimator**
```java
myView.animate().x(50f).y(100f);
```

更多关于**ViewPropertyAnimator**的信息，请参考Android Developers的[博客](http://android-developers.blogspot.com/2011/05/introducing-viewpropertyanimator.html).

<br>
# 在xml文件中定义动画
---

属性动画系统允许你不直接写代码而使用xml来声明定义属性动画。通过在xml文件中定义动画，可以让动画更容易在多个activity中重复使用，也能让编辑动画序列更加容易。  

为了区分使用了新属性动画API的动画文件和旧视图动画框架的动画文件，从android 3.1开始，你必须将使用属性动画的xml文件保存在__res/animator/__文件夹下。  

下面列举了属性动画类对应的xml标签：

* ValueAnimator - &lt;animator>
* ObjectAnimator - &lt;objectAnimator>
* AnimatorSet - &lt;set>

下面的示例顺序顺序执行两个动画组，第一个内嵌的动画组同时执行两个ObjectAnimator动画：
```xml
<set android:ordering="sequentially">
    <set>
        <objectAnimator
            android:propertyName="x"
            android:duration="500"
            android:valueTo="400"
            android:valueType="intType"/>
        <objectAnimator
            android:propertyName="y"
            android:duration="500"
            android:valueTo="300"
            android:valueType="intType"/>
    </set>
    <objectAnimator
        android:propertyName="alpha"
        android:duration="500"
        android:valueTo="1f"/>
</set>
```

为了执行这个动画，你需要在你的代码中把xml动画资源实例化为AnimatorSet对象，然后在开始动画之前设定动画的目标对象。可以简单的调用**setTarget()**方法给一个目标对象设定AnimatorSet中的全部子动画。代码如下：
```java
AnimatorSet set = (AnimatorSet) AnimatorInflater.loadAnimator(myContext,
    R.anim.property_animator);
set.setTarget(myObject);
set.start();
```

更多关于定义属性动画的xml语法，请参考[Animation Resources](https://developer.android.com/guide/topics/resources/animation-resource.html#Property)。
