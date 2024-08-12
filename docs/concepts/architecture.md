---
sidebar_label: Architecture Overview
sidebar_position: 2
---

# Planckster Ecosystem - Architecture Overview

The Planckster Ecosystem consists of four interconnected components that work together to provide you with filtered, debugged, and processed data on topics relevant to your needs. This data is ready for you to explore, analyze, and utilize for various applications.

The four key components of the Planckster architecture are: 1) **Kernel Planckster**, 2) **Data Scrapers**, 3) **Lieutenant Planckster**, and 4) **Websat Planckster**. These elements collaborate to gather vast amounts of data from relevant web pages, process it to suit your specific requirements, and store it for you to download, analyze, and apply as needed. Ultimately, this empowers you to make well-informed decisions based on accurate and relevant information.

## Kernel Planckster
Kernel Planckster is the central data management system within the Planckster ecosystem. It performs two primary functions: 1) receiving and storing information provided by the scrapers or the client, and 2) retrieving this stored information for your analysis. It manages the Metadata Database, where the attributes of received data are recorded, and the Block Storage element, where the actual data is stored.

You can interact with Kernel Planckster to request the extraction of relevant data from the internet or to download stored data through the Lieutenant Planckster platform on Kubeflow.

For more information on Kernel Planckster and how to use Kernel Planckster REST API, refer to the [Kernel Planckster Guides](../category/kernel-planckster)

## Data Scrapers

A Data Scraper is a program that automatically extracts relevant data from websites. There are three scrapers that are already set up in the Planckster Ecosystem: a Telegram scraper, a Twitter scraper and a Sentinel scraper (in future versions, more scrapers can be added). These scrapers retrive information from its corresponding website and then send the information to the Kernel Planckster software, which registers it in the Metadata database and saves it in the Block Storage element. The scraper can also augment the data using AI and LLM algorithms before sending it to Kernel Planckster.

For more information on how the data scrapers work, refer to the [Data Scrapers core concept document](./scrapers.md)

## Lieutenant Planckster

Lieutenant Planckster is a pipeline builder hosted on the Kubeflow platform. Pipelines are where all the different components - including code and images stored remotely - are pulled together into a workflow. Lieutenant Planckster functions as the control center, allowing users to interact with or issue commands to Kernel Planckster. These commands, written in code, when executed in the pipeline executor, trigger Kernel Planckster to perform its tasks. For instance, you can run the pre-configured scraper pipeline in Kubeflow to activate the scrapers and simultaneously retrieve data from sources like Telegram, Twitter, and Sentinel, storing it in the Block Storage element. The key inputs you need to provide to execute a pipeline is the tracer_id and the job_id as these are key to create the path where your data will be storage in Kernel Planckster. 

For more information on Lieutenant Planckster, refer to the [Lieutenant Planckster Guides](../category/lieutenant-planckster)

## Webstat Planckster

After executing the commands to scrape and save data in Kernel Planckster, you can analyze and interact with it using Python SDK, Jypyter Notebooks or Websat. Websat is a web interface designed to facilitate interaction with the Planckster ecosystem, allowing you to visualize and download scraped data. It also enables you to create conversations with your data and engage with LLM agents, focusing on data analysis and visualization. The first version of Websat, introduced in November 2023, showcased the LLM's data analysis capabilities. The current version, now in Beta, demonstrates its enhanced features for data upload, download, and visualization.

## Architecture Overview

This is how the Planckster Ecosystem functions as an articulated system. At its core is Kernel Planckster, which manages the flow of information based on client commands. Kernel Planckster manages both the Metadata Database and the Block Storage element, where it stores and organizes the data it handles. Kernel Planckster, the Metadata Database and the Block Storage element can be seen as one integrated element as neither can exist without the other two. 

The ecosystem also includes scrapers for platforms like Telegram, Twitter, and Sentinel, responsible for retrieving data from their respective servers and transmitting it to Kernel Planckster.

The data scraping process and workflow are determined by the pipelines that clients can create and run on the Lieutenant Planckster Kubeflow platform. When a pipeline is executed in the pipeline executor, the scrapers begin retrieving data and sending it to Kernel Planckster in the specified sequence. Kernel Planckster then saves the data in a unique location according to the tracer_id and job_id the client defines. Finally, the client can use Websat Planckster to visualize, analyze, and download the scraped data.






