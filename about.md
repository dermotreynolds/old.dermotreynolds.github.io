---
layout: page
permalink: /about/index.html
title: Dermot Reynolds
tags: [Dermot, Reynolds, Cloud, Transformation]
imagefeature: fourseasons.jpg
chart: true
---
<figure>
  <img src="{{ site.url }}/images/DR.png" alt="Dermot Reynolds">
  <figcaption>Dermot Reynolds</figcaption>
</figure>

{% assign total_words = 0 %}
{% assign total_readtime = 0 %}
{% assign featuredcount = 0 %}
{% assign statuscount = 0 %}

{% for post in site.posts %}
    {% assign post_words = post.content | strip_html | number_of_words %}
    {% assign readtime = post_words | append: '.0' | divided_by:200 %}
    {% assign total_words = total_words | plus: post_words %}
    {% assign total_readtime = total_readtime | plus: readtime %}
    {% if post.featured %}
    {% assign featuredcount = featuredcount | plus: 1 %}
    {% endif %}
{% endfor %}


My name is **Dermot Reynolds**, and this is my personal blog where I document some of the less well known or understood aspects of Cloud.

I have over 25 years experience in IT from developer to Oracle DBA to Sysadmin to Architect to CTO.

I am an Azure Certified Solutions Architect and would regard myself an expert in designing and implementing innovative solutions. 

<img src="/images/Cloud-Engineering-01.png" alt="drawing" width="400px"/>