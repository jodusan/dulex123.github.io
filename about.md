---
layout: page
title: About
permalink: /about/
---

{:refdef: style="text-align: center;"}
![My picture](/docs/assets/me.webp){: width="250" }
{: refdef}

<style>

color-pallete {
    color-blue: #0086B3;
    color-green: #008080;
    color-white: #E9ECEF;
    color-red: #BD4147;
    color-blue2: #007BFF;
}

</style>


<div id="wrapper-tmline">
<section id="cd-timeline" class="cd-container">
  {% assign sorted = site.timepoints | reverse %}
  {% for item in sorted %}
    <div class="cd-timeline-block">
      <div class="cd-timeline-img {{item.icon}}">
        <img src="/assets/img/{{item.icon}}.svg" alt="Picture">
      </div> <!-- cd-timeline-img -->
      <div class="cd-timeline-content">
        <h2>{{item.header}}</h2>
        <p>{{item.content}}</p>
        <span class="cd-date">{{item.date | date_to_string }}</span>
      </div> <!-- cd-timeline-content -->
    </div> <!-- cd-timeline-block -->
  {% endfor %}
</section> <!-- cd-timeline -->

</div>
