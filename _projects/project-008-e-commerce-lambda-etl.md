---
layout: page
title: "#008 : E-Commerce Lambda ETL"
cover-img: /assets/img/project-008/e-commerce-alibaba-illustration.png
---

> #### Key tools/skills:
> - [E-commerce Public Dataset by Alibaba](https://www.kaggle.com/datasets/AppleEcomerceInfo/ecommerce-information/data) as a data source
> - Python (Pandas, dbt) as a data transformation tool
> - Python-Kafla as a data stream processing
> - Apache Airflow as a data pipeline orchestration tool
> - Docker as containerization tool
> - BigQuery as a data warehouse
>
> Project : [_https://github.com/nairkivm/e-commerce-public-alibaba-etl_](https://github.com/nairkivm/e-commerce-public-alibaba-etl)

<br>

In the world of data engineering, the Lambda architecture stands out as a robust framework for processing large volumes of data. This architecture combines both batch and stream processing to handle real-time and historical data efficiently. In this blog post, we’ll explore an ETL project tailored for an Alibaba e-commerce platform, showcasing how Lambda architecture can be implemented using various tools and technologies.

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-system-diagram.png" alt="System design of this project, adopting Lambda Architecture" style="width:100%;">
    <figcaption style="font-size: 0.8em;">System design of this project, adopting Lambda Architecture.</figcaption>
</figure>

## Part I: Batch Processing

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-sales-data.png" alt="Entity relationship diagram of the 'sales' data in the destination schema" style="width:100%;">
    <figcaption style="font-size: 0.8em;">Entity relationship diagram of the 'sales' data in the destination schema.</figcaption>
</figure>

The batch processing component of this ETL project is designed to handle “sales” data. Using Pandas, we extract data from text files, transform it, and load it into Google BigQuery. This process ensures that large volumes of historical data are processed efficiently, providing a solid foundation for business analytics. For every step in this process, there is a data validation method so that the final result must meet all data requirements.

- Extraction: Data is extracted from text files containing historical sales records.

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-source-erd.png" alt="Entity relationship diagram of the source data" style="width:100%;">
    <figcaption style="font-size: 0.8em;">Entity relationship diagram of the source data.</figcaption>
</figure>

- Transformation: Using Pandas, the data is cleaned, aggregated, and transformed to meet the analytical requirements.

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-transformation-log.png" alt="Example of transform subprocess log" style="width:100%;">
    <figcaption style="font-size: 0.8em;">Example of transform subprocess log.</figcaption>
</figure>

- Loading: The transformed data is then loaded into Google BigQuery, where it can be queried and analyzed.

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-airflow-dag.png" alt="The DAG of overall ETL process to 'fact_order_details' table" style="width:100%;">
    <figcaption style="font-size: 0.8em;">The DAG of overall ETL process to 'fact_order_details' table.</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-load-result.png" alt="Preview data of the load subprocess result" style="width:100%;">
    <figcaption style="font-size: 0.8em;">Preview data of the load subprocess result.</figcaption>
</figure>

## Part II: Stream Processing

The stream processing component focuses on real-time data, specifically “customer cart” and “vendor availability” data. By leveraging Kafka, we can broadcast topics and process data in real-time. This allows vendors to monitor their product availability dynamically.

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-cart-and-availaibility-data.png" alt="Entity relationship diagram of the 'customer cart' and 'vendor availability' data in the destination schema" style="width:100%;">
    <figcaption style="font-size: 0.8em;">Entity relationship diagram of the 'customer cart' and 'vendor availability' data in the destination schema.</figcaption>
</figure>

Real-time data from various sources such as customer interactions and vendor updates is simulated using Kafka Producer and then ingested using Kafka Consumer. Every single message or data is then processed with Python utilities (and Pandas) and loaded into BigQuery, ensuring that it is available for immediate analysis and action. Vendors can monitor product availability and customer behavior in real-time, enabling quick decision-making.

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-kafka-log.png" alt="Example of stream process log" style="width:100%;">
    <figcaption style="font-size: 0.8em;">Example of stream process log.</figcaption>
</figure>

## Part III: Integrating Lambda Architecture and dbt

Combining batch and stream processing, this ETL project embodies the Lambda architecture. This dual approach allows us to handle both historical and real-time data seamlessly. The transformed data can be directly queried in the data warehouse platform or integrated with BI tools. This data provides actionable insights, helping businesses make informed decisions. Further transformation is achieved using dbt (data build tool), which refines the data for advanced analytics and reporting or simply serving data for the "data market".

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-docker.png" alt="Overall images on the container that run batch process (Apache Airflow and dbt services) and stream process (Zookeeper, Kafka, producer, and etl-stream)" style="width:100%;">
    <figcaption style="font-size: 0.8em;">Overall images on the container that run batch process (Apache Airflow and dbt services) and stream process (Zookeeper, Kafka, producer, and etl-stream).</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-dbt-model.png" alt="The dbt models implemented in this project as an extension after data load process. The dbt is also integrated with the Apache Airflow orchestration." style="width:100%;">
    <figcaption style="font-size: 0.8em;">The dbt models implemented in this project as an extension after data load process. The dbt is also integrated with the Apache Airflow orchestration.</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/img/project-008/e-commerce-alibaba-userrevenue-data.png" alt="The 'user_revenue' table which is the result of the dbt transformation is an example of the data served in the data market." style="width:100%;">
    <figcaption style="font-size: 0.8em;">The 'user_revenue' table which is the result of the dbt transformation is an example of the data served in the data market.</figcaption>
</figure>

## Conclusion

Implementing Lambda architecture in an ETL project offers a powerful way to manage both historical and real-time data. By combining batch processing with stream processing and leveraging tools like Pandas, Kafka, and dbt, we can create a robust data pipeline that meets the needs of modern data-driven businesses. This approach not only ensures efficient data processing but also provides valuable insights that drive business growth.

> Posted at 2024-09-25