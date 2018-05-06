---
layout: page
title: Tags
landing-title: 'Tags'
nav-menu: true
description: Collection of all tags.
image: null
author: null
---
<!-- Collect Tags -->
{% if site.tags != ""}
    {% include collecttags.html %}
{% endif %}

<!-- Main -->
<div id="main" class="alt">
    <!-- One -->
    <section id="one">
        <div class="inner">
            {% for tag in site.tags %}
                <a href="/tags/{{tag}}" class="button special small">{{tag}}</a>
            {% endfor %}
        </div>
    </section>
</div>