---
title: 判定表图例
date: 2019-05-05 13:06:55
tags: 软件测试

---

### 第6列

#### (1) 输入条件的自然语义陈述；

输入11010，表示C1 售货机可找零、C2 投入1元硬币、C4 按下橙汁按钮。

#### (2) 输出结果的自然语义陈述；

输出00110，表示E23 退还5角硬币、E24 送出橙汁饮料。

#### (3) 用命题逻辑形式描述实现上述输入-输出过程所应用的判定规则，并写出获得输出结果的推理演算过程。

![1557041303709](assets/1557041303709.png) 

以C1, C2, C4为前提，应用上述规则，可以证明逻辑结论E23 和E24。 

### 第23列

#### (1) 输入条件的自然语义陈述；

输入01001，表示C1 售货机无零钱找、C2 投入1元硬币、C5 按下啤酒按钮。

#### (2) 输出结果的自然语义陈述；

输出11000，表示E21 零货机零钱找完灯亮、E22 退还1元硬币。

#### (3) 用命题逻辑形式描述实现上述输入-输出过程所应用的判定规则，并写出获得输出结果的推理演算过程。

![1557041727475](assets/1557041727475.png) 

以非C1, C2, C5为前提，应用上述规则，可以证明逻辑结论E21 和E22。 

