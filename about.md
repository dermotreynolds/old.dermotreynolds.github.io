---
layout: page
permalink: /about/index.html
title: Dermot Reynolds
tags: [Dermot, Reynolds, Cloud, Transformation]
imagefeature: fourseasons.jpg
chart: true
---


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

### Hi!

My name is **Dermot Reynolds**, and this is my personal blog where I document some of the less well known or understood aspects of Cloud.

I have over 25 years experience in all aspects of IT.  I have been a developer, an Oracle DBA, a Sysadmin, a Solutions Architect and most recently a CTO.

I am an Azure Certified Solutions Architect and would regard myself as an expert in designing and implementing innovative Cloud solutions.

Hopefully, you will find some of this useful.

Thanks

Dermot

<figure>
  <img src="{{ site.url }}/images/exam-535-architecting-microsoft-azure-solutions-small.png" alt="Dermot Reynolds">
  <figcaption>Dermot Reynolds</figcaption>
</figure>