---
layout: post
title: Jekyll, GitHub и YouTube
date: 2016-03-26 02:00:00 +0300
tags: [ jekyll, github, youtube, trick ]

---

Как извесно, GitHub не поддерживает сторонние плагины при генерации страничек движка Jekyll.
По этому всякие штуки типа "jekyll-youtube" не работают, а хотелось бы.

<!--break-->

Как говориться, гугл в помощь! В итоге я нашел вот такое интересное решение:

* Создаем в \_includes файл youtube.html:
{% highlight html %}
<div class="video-container">
  <iframe width="560" height="315"
    src="//www.youtube.com/embed/{{ include.id }}"
    frameborder="0"
    allowfullscreen>
  </iframe>
</div>
{% endhighlight %}
* В теле поста пишем так:
{% highlight html %}
{% raw %}
{% include youtube.html id="Fo6aKnRnBxM" %}
{% endraw %}
{% endhighlight %}
* _(необязательно)_ Что бы видео автоматически ресайзилось при изменении размеров
окна браузера, необходимо добавить следующие строки в один из подключаемых файлов css
{% highlight css %}
.video-container {
  position: relative;
  padding-bottom: 56.25%;
  padding-top: 30px;
  height: 0;
  overflow: hidden;
}

.video-container iframe,
.video-container object,
.video-container embed {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
{% endhighlight %}

В итоге на страничке появится встроенное видео с YouTube:

{% include youtube.html id="Fo6aKnRnBxM" %}
