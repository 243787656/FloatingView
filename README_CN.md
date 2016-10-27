<p align="center">
  <a href="https://dribbble.com/shots/2583144-Aviate-Badge" target="_blank">
  <img width="320" height="240" src="https://github.com/UFreedom/FloatingView/blob/master/images/floating_anim.gif">
  </a>
 </p>

<h3 align="center">FloatingView</h3>

<p align="center">
  FloatingView can make the target view  floating above the anchor view with cool animation
</p>

<p align="center">
<a target="_blank" href="https://github.com/UFreedom/FloatingView"><img src="https://travis-ci.org/UFreedom/FloatingView.svg?branch=master"></a>
<a target="_blank" href="https://bintray.com/ufreedom/maven/FloatingViewLibrary/_latestVersion"><img src="https://api.bintray.com/packages/ufreedom/maven/FloatingViewLibrary/images/download.svg"></a>
<a target="_blank" href="https://github.com/UFreedom/FloatingView"><img src="https://img.shields.io/badge/android-3.0-brightgreen.svg" ></a>
<a target="_blank" href='https://github.com/UFreedom/FloatingView/blob/master/LICENSE.txt'><img src='https://img.shields.io/crates/l/rustc-serialize.svg'  /></a>
</p>

Usage
-----

### Step 1

在 build.gradle 文件中添加库依赖

```groovy
    dependencies {
        compile 'com.ufreedom.uikit:FloatingViewLib:1.0.0'
    }

```



### Step 2

使用 [FloatingBuilder][1] 创建一个 [FloatingElement][2]

```java

    FloatingElement builder = new FloatingBuilder()
                            .anchorView(View)
                            .target(View)
                            .offsetX(int)
                            .offsetY(int)
                            .floatingTransition(FloatingTransition)
                            .build();

```

使用 FloatingBuilder 可以设置的有
* anchorView ：锚点，也就是你想在哪个 View 上面进行漂浮动画
* target：目标，你想漂浮的 View
* offsetX：x 方向的偏移量，单位 px
* offsetY: y 方向的偏移量，单位 px
* floatingTransition : 漂浮效果，默认是 ScaleFloatingTransition

### Step 3 

创建一个 [Floating][3] 作为 [FloatingElement][2] 的容器,然后让你的 View 飞起来
 
```java
    Floating floating = new Floating(getActivity());
    floating.startFloating(builder);
```
 

自定义
-----


####坐标系

 <img src="http://oeapkptbn.bkt.clouddn.com/coordinate.png" width="572" height="427"/>


####类图

 <img src="http://oeapkptbn.bkt.clouddn.com/classdiagram.png" width="831" height="428"/>



####漂浮动画 

实现漂浮动画很简单，你只需要实现 [FloatingTransition][4] 就可以:

```java

    public interface FloatingTransition {
        public void applyFloating(YumFloating yumFloating);
    }

```

在 `applyFloating` 方法内，你可以是 Android Animation 创建动画，然后使用 YumFloating 进行 Alpha,Scale,Translate,Rotate 等变换

如果你想加入 [Facebook Rebound][5] 回弹动画效果，你可以使用 SpringHelper,例如 ScaleFloatingTransition:

```java
    public class ScaleFloatingTransition implements FloatingTransition {

    ...
    
    @Override
    public void applyFloating(final YumFloating yumFloating) {
        
        ValueAnimator alphaAnimator = ObjectAnimator.ofFloat(1.0f, 0.0f);
        alphaAnimator.setDuration(duration);
        alphaAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                yumFloating.setAlpha((Float) valueAnimator.getAnimatedValue());
            }
        });
        alphaAnimator.start();

        SpringHelper.createWidthBouncinessAndSpeed(0.0f, 1.0f,bounciness, speed)
                .reboundListener(new SimpleReboundListener(){
                    @Override
                    public void onReboundUpdate(double currentValue) {
                        yumFloating.setScaleX((float) currentValue);
                        yumFloating.setScaleY((float) currentValue);
                    }
                }).start(yumFloating);
    }
    
}


```

如果 
 
 
####漂浮路径动画 

 
 
 
License 
--------

    Copyright 2015 UFreedom

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

[1]:https://github.com/UFreedom/FloatingView/blob/master/FloatingViewLib/src/main/java/com/ufreedom/floatingview/FloatingBuilder.java
[2]:https://github.com/UFreedom/FloatingView/blob/master/FloatingViewLib/src/main/java/com/ufreedom/floatingview/FloatingElement.java
[3]:https://github.com/UFreedom/FloatingView/blob/master/FloatingViewLib/src/main/java/com/ufreedom/floatingview/Floating.java
[4]:https://github.com/UFreedom/FloatingView/blob/master/FloatingViewLib/src/main/java/com/ufreedom/floatingview/transition/FloatingTransition.java
[5]:http://facebook.github.io/rebound/