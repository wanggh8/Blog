---
title: NestedScrollView
date: 2020-06-15 10:31:12
tags: Android


---

<!--more-->

 因此，我们可以简单的把 NestedScrollView类比为 ScrollView，其作用就是作为控件父布局，从而具备（嵌套）滑动功能。

**[NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView)** 与 [ScrollView](https://developer.android.com/reference/android/widget/ScrollView.html) 的区别就在于 **[NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView)** 支持 *嵌套滑动*，无论是作为父控件还是子控件，嵌套滑动都支持，且默认开启。

在一些需要支持嵌套滑动的情景中，比如一个 [ScrollView](https://developer.android.com/reference/android/widget/ScrollView.html) 内部包裹一个 `RecyclerView`，那么就会产生滑动冲突，这个问题就需要你自己去解决。而如果使用 **[NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView)** 包裹 `RecyclerView`，嵌套滑动天然支持，你无需做什么就可以实现前面想要实现的功能了。

我们通常为`RecyclerView`增加一个 Header 和 Footer 的方法是通过定义不同的 viewType来区分的，而如果使用 **[NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView)**，我们完全可以把`RecyclerView`当成一个单独的控件，然后在其上面增加一个控件作为 Header，在其下面增加一个控件作为 Footer。

