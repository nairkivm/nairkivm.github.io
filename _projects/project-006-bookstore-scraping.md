---
layout: page
title: "#006 : Bookstore Scraping"
cover-img: /assets/img/project-006/bookstore-scraping.jpeg
---

> #### The platform/tools used:
> - Data cleaning & manipulation : Pandas
> - Database System : SQLite
> - Container : Docker
>
> Project : [_https://github.com/nairkivm/book-scrape_](https://github.com/nairkivm/book-scrape)

<br>

One day, you are hired to gather information about books that your uncle wants to sell at his new bookstore. As you delve into this task, you begin collecting data on book titles, prices, and other relevant details. However, you don't want to engage in the tedious process of copying and pasting back and forth between the web and your spreadsheet, do you?

Don't worry, I got your back! I built this project for you to automatically fetch all the data that you want and store the data in an ```SQLite``` database with just one click!

<figure>
    <img src="/assets/img/project-006/project-006-workflow.png" alt="project-006-workflow" style="width:100%;">
    <figcaption>Workflow of this project.</figcaption>
</figure>

I used ```requests``` and ```Beautiful Soup``` module to get the raw HTML data and parse it into meaningful data. To organize and format the data, I used ```Pandas``` module before inserting it in the database.

Here's the [repository](https://github.com/nairkivm/book-scrape) of this project. Good luck with your uncle's new bookstore!

<figure>
    <img src="/assets/img/project-006/data-sample.png" alt="project-006-data-sample" style="width:100%;">
    <figcaption>Data sample.</figcaption>
</figure>
