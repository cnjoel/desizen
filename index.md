---
layout: home
title: 	less.im
description: #simplelife #ror #html #css
---
<section class="main" role="main">
    <ul class="article-list">
{% for post in site.posts %}
      <li>
        <article class="article">
          <header class="article-header">
            <h1 class="article-title"><a class="article-link" href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>
            <time class="article-date">{{ post.date | date_to_string }}</time>
          </header>
        </article>
      </li>  
{% endfor %}
    </ul>
</section>