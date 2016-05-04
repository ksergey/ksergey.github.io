---
layout: post
title: Создание объектов без копирования в std::map
date: 2016-05-04 18:50:00 +0300
tags: [ cpp ]
sitemap: false
comments: true

---

Вы когда-нибудь пытались создать объект (value) в std::map-е без использования std::move?
До сегодняшнего дня я не знал как это сделать.

![КДПВ]({{ site.url }}/public/post-img/kdpv0.jpg){:.center-image}

<!--break-->

В c++11 у std::map есть метод emplace с такой сигнатурой:

{% highlight cpp %}
template< class... Args >
std::pair<iterator,bool> emplace( Args&&... args );
{% endhighlight %}

Вроде бы как inplace создание объектов возможно, но value\_type в std::map есть std::pair< key\_type, mapped\_type >.
Т.е. что бы сконструировать объект в std::map мы должны этот объект уже создать извне и как минимум будет вызван
конструктор перемещения для объекта.

## Так все же можно ли создать объект в std::map без вызова конструктора перемещения/копирования?

Ответ да, можно! Для этого нам потребуется замечательная константа [std::piecewise\_construct](http://en.cppreference.com/w/cpp/utility/piecewise_construct).

Пример:

{% gist ksergey/a2f046b2f6b4fc7a45e8e5a62980eb58 map_forward.cpp %}

Результат выполнения программы:

{% gist ksergey/a2f046b2f6b4fc7a45e8e5a62980eb58 output %}

Как видно из примера, для 3-го случая вызывается только один конструктор.

## Где это может пригодиться?

Лично для себя этот трюк я использую для объектов, которые не имеют конструктора перемещения или конструктора копирования.
Без std::piecewise\_construct пришлось бы объект оборачивать в какой-нибудь std::unique\_ptr, что добавляет оверхеда к обращения к
объекту.
