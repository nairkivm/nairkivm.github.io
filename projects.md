---
layout: page
title: Projects
full-width: true
---

<style>
/* This is the CSS section */
.container {
  /* This is the container for the whole page */
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}
.projects {
  /* This is the container for the projects */
  display: flex; /* This makes the projects align horizontally */
  flex-wrap: wrap; /* This makes the projects wrapped when exceeding maximum width */
  overflow-y: auto; /* This makes the projects scrollable vertically */
  width: 110%;
  height: 500px;
  margin: 10px;
  padding: 50px;
  gap: 40px;
  scroll-behavior: smooth; /* This makes the scrolling smooth */
  white-space: nowrap;
}
.project {
  /* This is the container for each project */
  flex: 0 0 calc(27%);
  height: 200px;
  margin: 10px;
  border: 1px solid black;
  box-shadow: 5px 5px 5px grey;
}
.project img {
  /* This is the image for each project */
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.project p {
  /* This is the text for each project */
  font-size: 11px;
  text-align: center;
  padding: 0 calc(20%);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  width: 300px;
}
</style>

<div class="container">
  <div class="projects">
    <!-- This is where you add your projects -->
    {% for project in site.projects %}
    <div class="project">
        {%- capture thumbnail -%}
            {% if project.thumbnail-img %}
                {{ project.thumbnail-img }}
            {% elsif project.cover-img %}
                {% if project.cover-img.first %}
                    {{ project.cover-img[0].first.first }}
                {% else %}
                    {{ project.cover-img }}
                {% endif %}
            {% else %}
            {% endif %}
        {% endcapture %}
        {% assign thumbnail=thumbnail | strip %}
        <a href="{{ project.url | absolute_url }}" aria-label="Thumbnail">
            <img src="{{ project.cover-img | absolute_url }}" alt="Project Thumbnail">
        </a>
        <p>{{ project.title | strip_html }}</p>
    </div>
    {% endfor %}
  </div>
</div>

