---
title: "아무도 안 알려주는 개발 잡학사전"
layout: archive
permalink: categories/devdictionary
author_profile: true
sidebar_main: true
---

***

{% assign posts = site.categories['Dev Dictionary'] %}
{% for post in posts %} 
{% include archive-single2.html type=page.entries_layout %} 
{% endfor %}