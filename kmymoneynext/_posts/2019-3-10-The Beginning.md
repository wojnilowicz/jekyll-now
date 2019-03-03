---
layout: post
title: The Beginning
---

I've decided to fork [KMyMoney](https://cgit.kde.org/kmymoney.git/) and name it KMyMoneyNEXT. There are two reasons for that:

1. I think that long release schedules of KMyMoney are not good for me,
2. I also think, that keeping vital code in [Alkimia](https://cgit.kde.org/alkimia.git/) library is not good.

What I would like to achive is to allow buidling KMyMoney with as few components as possible and put every extension in plugins. I hope that this way I could run it on an Android device some day in the future.

As for technical side, there is already no compatibility between KMyMoney and KMyMoneyNEXT storages - I improved storing of merged transactions.