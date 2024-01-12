---
layout: page
title: "#004 : Weather Forecast"
cover-img: /assets/img/project-004/ilustrasi-cuaca-kupang-ntt.jpeg
---

> #### The tools that I used:
> - Extract data : Google Sheets
> - Workflow Management : Apache Airflow
> - Data cleaning & manipulation : Pandas
> - Database Connection : Psycopg2
> - Database System : PostgreSQL
> - Container : Docker
> - App Deployment & Dashboard : Streamlit
>
> Project : [_https://github.com/nairkivm/weather-forecast_](https://github.com/nairkivm/weather-forecast)

<br>

<style>
figure {
  display: inline-block;
  text-align: center
}
</style>

<figure>
    <img src="/assets/img/project-004/weather-forecast-architecture.drawio.png"
         alt="weather-forecast-architecture" style="width:100%;">
    <figcaption>weather-forecast Architecture.</figcaption>
</figure>

This is an end-to-end data engineering project that extracts weather forecast data from [BMKG](https://www.bmkg.go.id/en.html) (Indonesian agency for meteorology, climatology, and geophysics), stores it in a local database, and feeds the data into a dashboard. I built this entirely in [Python](https://www.python.org/). 

To extract the data, I wanted to use [BMKG's API](https://api.bmkg.go.id/prakiraan-cuaca), but it was blocked and their [website](https://data.bmkg.go.id/prakiraan-cuaca/) prevented scraping. So, I found a workaround using ["IMPORTHTML"](https://support.google.com/docs/answer/3093339?hl=en) and ["IMPORTXML"](https://support.google.com/docs/answer/3093342?hl=en) formulas from Google Sheets (I don't know why web scraping doesn't work, but this Google Sheets formula trick does). Here's the [link](https://docs.google.com/spreadsheets/d/1DZdXexhve4Bih_MYW04PYrbKum8luXvgCZBucWtnFf4/copy) to copy the Google Sheets file.

<figure>
    <img src="/assets/img/project-004/weather-forecast-dag.png"
         alt="weather-forecast-dag" style="width:100%;">
    <figcaption>weather-forecast DAG.</figcaption>
</figure>

After I extracted the data, I stored the raw data into a data warehouse. I was afraid to use [S3](https://aws.amazon.com/s3/) storage (because they can charge me if I'm careless), so I just stored the data locally. And then, I processed the raw data into clean data, heavily utilizing the [pandas](https://pandas.pydata.org/) module, and stored it in a database (that also ran on my local PostgreSQL server). I used the [psycopg2](https://www.psycopg.org/docs/) module to connect [PostgreSQL](https://www.postgresql.org/docs/) and Python. I created a scheduled workflow of extract-load-transform data daily using [Apache Airflow](https://airflow.apache.org/docs/), run inside a [Docker](https://docs.docker.com/) container. 

<figure>
    <img src="/assets/img/project-004/weather-forecast-erd.png"
         alt="weather-forecast-erd" style="width:100%;">
    <figcaption>weather-forecast ERD for the database.</figcaption>
</figure>

After that, I used [Streamlit](https://docs.streamlit.io/), an app framework in Python, to display a dashboard showing the weather forecast and the temperature of Kab. Kupang, NTT. Initially, I wanted to connect the app with the database, but I ran my database locally, so it was unsafe and unreliable. For displaying purposes, the data source came from this project directory.

<style>
  #wrap {
    width: 750px;
    height: 1500px;
    padding: 0;
    overflow: hidden;
  }
  #scaled-frame {
    width: 1000px;
    height: 2000px;
    border: 0px;
  }
  #scaled-frame {
    zoom: 0.75;
    -moz-transform: scale(0.75);
    -moz-transform-origin: 0 0;
    -o-transform: scale(0.75);
    -o-transform-origin: 0 0;
    -webkit-transform: scale(0.75);
    -webkit-transform-origin: 0 0;
  }
  @media screen and (-webkit-min-device-pixel-ratio:0) {
    #scaled-frame {
      zoom: 1;
    }
  }
</style>

<div id="wrap">
  <iframe id="scaled-frame"
    src="https://nairkivm-weather-forecast-weather-forecast-dashboard.streamlit.app/?embed=true"
    style="width:100%;"
  ></iframe>
</div>

Trivia: 
1. This was just a sample project, so the dataset was small. I deliberately chose a location and omitted other metrics, like humidity, wind direction, etc. 
2. I chose Kab. Kupang, NTT because that is a city where the Indonesian National Observatory (at Mount Timau) takes place, so this dashboard could be used to track the weather for astronomy observation purpose.