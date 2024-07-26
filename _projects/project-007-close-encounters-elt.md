---
layout: page
title: "#007 : Close Encounters ELT"
cover-img: /assets/img/project-007/close-encounters-illustration.png
---

> #### Key tools/skills:
> - Using JPL NASA's SBDB Close-Approach Data API as a data source
> - Python (Pandas) as a data transformation tool
> - PostgreSQL as a data warehouse (via Koyeb)
> - Luigi as a data pipeline orchestration tool
> - Power BI as data visualization tool
>
> Project : [_https://github.com/nairkivm/close-encounters-elt-project_](https://github.com/nairkivm/close-encounters-elt-project)

<br>

In this project, I'm demonstrating my skills to make an end-to-end data pipeline, orchestrated with [Luigi](https://luigi.readthedocs.io/en/stable/), and visualised the result using [Power BI](https://www.microsoft.com/en-us/power-platform/products/power-bi/downloads). The objective of this project is to gain some insights from small bodies, like asteroid and comets, that is approaching the Earth and managed to encounter the Earth within a relatively very small distance.

I used JPL's SBDB (Small Bodies Database) Close Approach Data API as the data source. The analysis is limited to discovered objects that resides within 0.05 au radius from the Earth spanning from 2019 to 2023. This is a data preview after the data processing.

<figure>
    <img src="/assets/img/project-007/pgadmin-screenshot-1721875797027.png" alt="close-encounters-preview-data" style="width:100%;">
    <figcaption>Close encounters data preview.</figcaption>
</figure>

Because I'm extracting the historical data, I don't use _cron_ to schedule the run of data workflow. If the data that we want to extract is updated regularly, we can use _cron_ to trigger _central scheduler_ of our Luigi task. We can choose between _local scheduler_ or _central scheduler_ to run our workflow, but for the production usage, we should use central scheduler. With the latter, we can also access the Luigi UI to visualize and monitor our data workflow. Below are some screenshoot of Luigi UI that is used in this project.

<figure>
    <img src="/assets/img/project-007/luigiui-screenshot-1721875797024.png" alt="luigi-ui-1-of-3" style="width:100%;">
    <figcaption>Home of the UI.</figcaption>
</figure>


<figure>
    <img src="/assets/img/project-007/luigiui-screenshot-1721875797025.png" alt="luigi-ui-2-of-3" style="width:100%;">
    <figcaption>Task dependency graph.</figcaption>
</figure>


<figure>
    <img src="/assets/img/project-007/luigiui-screenshot-1721875797026.png" alt="luigi-ui-3-of-3" style="width:100%;">
    <figcaption>Task dependency graph in another style.</figcaption>
</figure>

I'm focusing the analysis on the close approach timestamp, object counts, close approach distance, and the diameter of the objects. There is a total number of 7605 objects that undergone close encounter with Earth and was discovered from the last five years. It is about 1500 objects discovery per year, but the actual trends were increasing (excluding 2022-2023 case). The improvements of more advanced instruments to detect smaller and dimmer objects maybe contributes to this trend. However, only 8% of them were actually traversing within Earth-Moon distance radius and maybe posing a considerable alert, especially if the diameter size is about a half of the height of Mount Fuji, Japan (1.88 km).

<figure>
    <img src="/assets/img/project-007/close-encounters-2-1.png" alt="analysis-result" style="width:100%;">
    <figcaption>Analysis result: a Power BI Dashboard titled "Cosmic Trespassers: Natural Small Bodies Closely Encountered with Earth.</figcaption>
</figure>

When I did further analysis on the discovery trends, I found an interesting pattern. It seems that there is a consistent annual pattern of the discovery that suggest the number peaked at October and plunged on July. I still don't know the underlaying mechanism going on there, whether it's an actual near-Earth objects orbital phenomenon or effect of biased observation.