---
layout: page
title: Projects
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
.title {
  /* This is the title of the page */
  font-size: 36px;
  text-align: center;
  margin: 20px;
}
.projects {
  /* This is the container for the projects */
  display: flex; /* This makes the projects align horizontally */
  overflow-x: auto; /* This makes the projects scrollable horizontally */
  width: 100%;
  height: 300px;
  margin: 10px;
  padding: 10px;
  scroll-behavior: smooth; /* This makes the scrolling smooth */
}
.project {
  /* This is the container for each project */
  width: 200px;
  height: 200px;
  margin: 10px;
  padding: 10px;
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
  font-size: 18px;
  text-align: center;
}
</style>

<div class="container">
  <h1 class="title">Projects</h1>
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
        {% if site.feed_show_excerpt == false %}
        {% if thumbnail != "" %}
        <a href="{{ project.url | absolute_url }}" aria-label="Thumbnail">
            <img src="{{ thumbnail | absolute_url }}" alt="Project Thumbnail">
        </a>
        <p>{{ project.title | strip_html }}</p>
        {% endif %}
        {% endif %}
    </div>
    {% endfor %}
  </div>
</div>

