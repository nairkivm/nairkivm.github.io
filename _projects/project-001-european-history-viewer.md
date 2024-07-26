---
layout: page
title: "#001 : European\nHistory Viewer"
cover-img: https://genealogy.webonizer.net/media/1/image/jpeg/original/17_europe_13th_century.jpg
---

I’ve been playing a lot of [Rise of Nations](https://en.wikipedia.org/wiki/Rise_of_Nations) lately, and it’s gotten me interested in world history, particularly the European. I also wanted to show off my data skills, so I made a cool thing called the European History Viewer. It’s a dashboard that shows you what happened in Europe over time and where the event(s) took place, and I got all the data from [Wikipedia](https://en.wikipedia.org/wiki/History_of_Europe). Pretty neat, huh? 😎

![European History Viewer](/assets/img/project-001/GAjB3zba0AA8Yns.jpeg){: .mx-auto.d-block :}

This might be very simple and unimpressive. I just used Google Sheets, the data is not complex, the dashboard is very simple, and the display is not attractive. Can this be called a data project?? 

<video width="720" controls>
  <source src="/assets/img/project-001/european_viewer_demo.mp4" type="video/mp4">
</video>

Anyway, this is the framework of this project.
1. I extracted the data from a table in [History of Europe](https://en.wikipedia.org/wiki/History_of_Europe) page and [list of countries in the world](https://en.wikipedia.org/wiki/List_of_countries_by_the_United_Nations_geoscheme) page into a spreadsheet file using a Google Sheets formula, ```IMPORTHTML```
2. I put the data into its respective tables (sheets) inside a _"data warehouse"_ (because Spreadsheet is not really a database system!)
3. Using various formulas, I cleaned the data into more organized and comprehensible form. 
4. Because the list of history from the Wikipedia page has no information about the country(-es) where the historical event took place, I asked [Bing AI](https://www.bing.com/search?q=Bing+AI&showconv=1) to query that lost information. I store and cleaned the data into another table (sheet).
5. I _"joined"_ the list of history table and list of all countries using various formulas, especially ```FILTER```, ```MAP```, and ```ARRAYFORMULA```.
6. Finally, I made a _"dashboard"_ that contains a customizable table that can display any field of information with certain filters. I also added a map where the event took place.

![Framework of European History Viewer](/assets/img/project-001/GAjIEvMbYAA1T-e.png){: .mx-auto.d-block :}

That's all. If you are interested in this projects, you can copy it yourself from this [link](https://docs.google.com/spreadsheets/d/1c-iEcDJ1wJx8r8iwB6Y5f1HlQxAMoDByfCBmP8s4nE0/copy?usp=sharing)! 
