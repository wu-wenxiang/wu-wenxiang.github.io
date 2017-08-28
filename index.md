---
layout: home
---

<div class="index-content about">
    <div class="section">
        <ul class="artical-cate">
            <li class="on"><a href="/"><span>About</span></a></li>
            <li><a href="/blog"><span>Blog</span></a></li>
            <li><a href="/course"><span>Course</span></a></li>
            <li><a href="/project"><span>Project</span></a></li>
        </ul>

        <div class="cate-bar"><span id="cateBar"></span></div>

        <ul class="artical-list">
        {% for post in site.categories.about %}
            <li>
                <h2>
                    <a href="{{ post.url }}">{{ post.title }}</a>
                </h2>
                <div class="title-desc">{{ post.description }}</div>
            </li>
        {% endfor %}
        </ul>
    </div>
    <div class="aside">
    </div>
</div>
