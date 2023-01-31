---
title: "C# 으로 이해하는 자료구조"
layout: archive
permalink: categories/data-structure-1
author_profile: true
sidebar_main: true
---

<!-- 공백이 포함되어 있는 카테고리 이름의 경우 site.categories['a b c'] 이런식으로! -->

***

{% assign posts = site.categories['Data Structure 1'] %}
{% for post in posts %} 
{% include archive-single2.html type=page.entries_layout %} 
{% endfor %}