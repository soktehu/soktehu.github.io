---
layout: default
title: About Me
---

<img class="profile-picture" src="{{ site.baseurl }}/assets/images/profile.JPG" alt="Karina Tehusijarana">

# Welcome to Sok Tehu

Just a blog for all my bits and bobs.

---

<section class="essay-section">
    <h2>Recent Posts</h2>
    
    {% if site.posts.size > 0 %}
        <ul class="essay-list">
            {% for post in site.posts limit: 5 %}
                <li class="essay-item">
                    <div class="essay-meta">
                        <span class="essay-date">{{ post.date | date: "%B %d, %Y" }}</span>
                        <span class="essay-meta-separator">•</span>
                        <span class="essay-read-time">
                            {% assign words = post.content | number_of_words %}
                            {% if words < 360 %}
                                1 min read
                            {% else %}
                                {{ words | divided_by: 180 | plus: 1 }} min read
                            {% endif %}
                        </span>
                    </div>
                    <h3 class="essay-title">
                        <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
                    </h3>
                    <p class="essay-excerpt">
                        {% if post.description %}
                            {{ post.description }}
                        {% else %}
                            {{ post.excerpt | strip_html | strip_newlines | truncate: 150 }}
                        {% endif %}
                    </p>
                </li>
            {% endfor %}
        </ul>
    {% else %}
        <p class="no-essays">No essays posted yet. Watch this space!</p>
    {% endif %}
</section>

