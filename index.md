---
layout: page
title: Welcome aboard
tagline: beware of dragons
---
{% include JB/setup %}

#### Recent posts

<ul class="posts">
  {% for post in site.posts limit: 16 %}
    <li><span>{{ post.date | date: "%Y-%m-%d" }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
    {% if post.description %}<br><div class="info">{{ post.description }}</div>{% endif %}
    </li>
  {% endfor %}
</ul>

Browse by: [Categories](/categories.html), [Tags](/tags.html), [Archive](/archive.html).

#### Also worth reading

 - [Charlie Stross](http://www.antipope.org/charlie/blog-static/)
 - [Peter Watts](http://www.rifters.com/crawl/)
 - [Stanislav](http://www.loper-os.org/)
 - [Bronikowski](http://bronikowski.com/) (note: in Polish)

#### About the site

Permanent work in progress. Sharing ideas through non-assuming means.

This site builds on the following software:

 - [Jekyll Bootstrap](http://jekyllbootstrap.com/) - nice balance between features and complexity
 - [Twitter Bootstrap](http://twitter.github.com/bootstrap/) - because 'blog' design is overrated
 - [Bitcons Icons](http://somerandomdude.com/work/bitcons/) - for the 'logo' image, top-left corner
