---
layout: page
title: Search
permalink: /search/
---

<div id="search-container">
    <input type="text" id="search-input" placeholder="Search through the blog posts...">
    <ul id="results-container"></ul>
</div>

<script src="{{ site.baseurl }}/assets/simple-jekyll-search.min.js" type="text/javascript"></script>

<script>
    SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('results-container'),
    searchResultTemplate: '<div style="text-align: left !important;"><a href="{url}"><h1 style="text-align:left !important;">{title}</h1></a><span style="text-align:left !important;">{date}</span></div>',
    json: '{{ site.baseurl }}/search.json'
    });
</script>
<div class = "centermake">
  <div class = "frame">
      <div class="square" id="s0"></div>
      <div class="square" id="s1"></div>
      <div class="square" id="s2"></div>
      <div class="square" id="s3"></div>
      <div class="square" id="s4"></div>
      <div class="square" id="s5"></div>
      <div class="square" id="s6"></div>
      <div class="square" id="s7"></div>
      <div class="square" id="s8"></div>
      <div class="square" id="s9"></div>
      <div class="circle" id="circle0" ></div>
  </div>
</div>
