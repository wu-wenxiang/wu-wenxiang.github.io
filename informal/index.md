---
layout: home
---

<div class="index-content informal">
    <div class="section">
        <ul class="artical-cate">
            <li><a href="/"><span>Blog</span></a></li>
            <li><a href="/course"><span>Course</span></a></li>
            <li><a href="/project"><span>Project</span></a></li>
            <li><a href="/essay"><span>Essay</span></a></li>
            <li class="on"><a href="/informal"><span>Informal</span></a></li>
        </ul>

        <div class="cate-bar"><span id="cateBar"></span></div>

        <ul class="artical-list">
        {% for post in site.categories.informal %}
            <li>
                <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
                <div class="title-desc">{{ post.description }}</div>
            </li>
        {% endfor %}
        </ul>
    </div>
    <div class="aside">
    </div>
</div>
