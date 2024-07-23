---
layout: main
---

## About

Hi! I am Utsav, currently a Master Student at [University of Stuttgart](https://www.uni-stuttgart.de/), Germany. 

I like to discuss about deep learning, large language models and sometimes mathematics.  
I will try to log my learnings through this blog.

Connect with me on:   
- X [@the5amperson](https://x.com/the5amperson).   
- Github [@theutsavpanchal](https://github.com/theutsavpanchal)
- mail [panchalutsav274@gmail.com](mailto:panchalutsav274@gmail.com)

Some of the tech leaders I read in my free time.  

- [Andrej Karpathy](https://karpathy.github.io/)  

- [Paul Graham's Essays](https://paulgraham.com/articles.html)       

- [Chip Huyen](https://huyenchip.com/)


---

## Recently Written

<ul class="related-posts">

{% assign blog_posts = site.posts | where: 'blog_post', true %}
{% for post in blog_posts limit:3 %}
    <li class="main-page-list">
        <h4>
            <div style="display: inline-block; width: 90px">
                <small>{{ post.date | date: "%Y-%m-%d" }}</small>
            </div>
            <div id="main-page-blogs-list">
                <a class="una" href="{{ site.baseurl }}{{ post.url }}">
                    <span>{{ post.title }}</span>
                </a>
            </div>
        </h4>
    </li>
    {% if forloop.last %}</ul>{% endif %}
{% endfor %}


## What I Read Recently

<ul class="related-posts">

{% assign book_reviews = site.posts | where: 'book', true %}
{% for post in book_reviews limit:7 %}
    <li class="main-page-list">
        <h4>
        <a class="una" href="{{ post.goodreads_url }}">
            <span>{{ post.title }}</span>
        </a>
            <small>by {{ post.author }}.</small>
            <small>published {{ post.year }}.</small>
        </h4>
    </li>
    {% if forloop.last %}</ul>{% endif %}
{% endfor %}
