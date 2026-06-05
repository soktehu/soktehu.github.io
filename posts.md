---
layout: default
title: Posts
permalink: /posts/
---

<div class="posts-intro">
    <h1>All Posts</h1>
    <p>A collection of essays, stories, and thoughts.</p>
</div>

<!-- Tags filter section -->
<div class="tags-container" aria-label="Filter posts by category">
    <button class="tag-filter-btn active" data-tag="all">All</button>
    {% comment %} Collect all unique tags and sort them {% endcomment %}
    {% capture tags_string %}{% for post in site.posts %}{% for tag in post.tags %}{{ tag }}|{% endfor %}{% endfor %}{% endcapture %}
    {% assign all_tags = tags_string | split: "|" | uniq | sort %}
    {% for tag in all_tags %}
      {% assign trimmed_tag = tag | strip %}
      {% if trimmed_tag != "" %}
        <button class="tag-filter-btn" data-tag="{{ trimmed_tag | downcase }}">{{ trimmed_tag }}</button>
      {% endif %}
    {% endfor %}
</div>

<!-- Posts list -->
<section class="essay-section">
    <ul class="essay-list" id="posts-list">
        {% for post in site.posts %}
            {% capture post_tags_json %}[{% for tag in post.tags %}"{{ tag | downcase }}"{% unless forloop.last %},{% endunless %}{% endfor %}]{% endcapture %}
            <li class="essay-item post-list-item" data-tags='{{ post_tags_json }}'>
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
                    {% if post.tags.size > 0 %}
                        <span class="essay-meta-separator">•</span>
                        <span class="essay-item-tags">
                            {% for tag in post.tags %}
                                <span class="essay-item-tag">#{{ tag }}</span>
                            {% endfor %}
                        </span>
                    {% endif %}
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
    <p id="no-posts-message" class="no-essays" style="display: none;">No posts found matching the selected category.</p>
</section>

<script>
    document.addEventListener('DOMContentLoaded', () => {
        const buttons = document.querySelectorAll('.tag-filter-btn');
        const items = document.querySelectorAll('.post-list-item');
        const noPostsMessage = document.getElementById('no-posts-message');

        function filterTag(selectedTag) {
            let matchedCount = 0;
            const normalizedTag = selectedTag.toLowerCase().trim();
            
            // Update buttons active state
            buttons.forEach(btn => {
                const btnTag = btn.getAttribute('data-tag').toLowerCase().trim();
                if (btnTag === normalizedTag) {
                    btn.classList.add('active');
                } else {
                    btn.classList.remove('active');
                }
            });

            // Filter items
            items.forEach(item => {
                const itemTags = JSON.parse(item.getAttribute('data-tags') || '[]');
                const normalizedItemTags = itemTags.map(t => t.toLowerCase().trim());
                
                if (normalizedTag === 'all' || normalizedItemTags.includes(normalizedTag)) {
                    item.style.display = '';
                    matchedCount++;
                } else {
                    item.style.display = 'none';
                }
            });

            // Update no posts message visibility
            if (matchedCount === 0) {
                noPostsMessage.style.display = '';
            } else {
                noPostsMessage.style.display = 'none';
            }
        }

        // Add event listeners to filter buttons
        buttons.forEach(button => {
            button.addEventListener('click', () => {
                const tag = button.getAttribute('data-tag');
                filterTag(tag);
                // Update URL search param without reloading
                const url = new URL(window.location);
                if (tag === 'all') {
                    url.searchParams.delete('tag');
                } else {
                    url.searchParams.set('tag', tag);
                }
                window.history.pushState({}, '', url);
            });
        });

        // Check URL parameters on load
        const params = new URLSearchParams(window.location.search);
        const urlTag = params.get('tag');
        if (urlTag) {
            filterTag(urlTag);
        }
    });
</script>
