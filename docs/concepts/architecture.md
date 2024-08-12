---
sidebar_label: Architecture Overview
sidebar_position: 2
---

# Planckster Ecosystem - Architecture Overview

The Planckster Ecosystem consists of four interconnected components that work together to provide you with filtered, debugged, and processed data on topics relevant to your needs. This data is ready for you to explore, analyze, and utilize for various applications.

The four key components of the Planckster architecture are: 1) **Kernel Planckster**, 2) **Data Scrapers**, 3) **Lieutenant Planckster**, and 4) **Websat Planckster**. These elements collaborate to gather vast amounts of data from relevant web pages, process it to suit your specific requirements, and store it for you to download, analyze, and apply as needed. Ultimately, this empowers you to make well-informed decisions based on accurate and relevant information.

## Kernel Planckster
Kernel Planckster is the central data management system within the Planckster ecosystem. It performs two primary functions: 1) receiving and storing information provided by the scrapers or the client, and 2) retrieving this stored information for your analysis. It manages the metadata database, where the attributes of received data are recorded, and the Block Storage element, where the actual data is stored.

You can interact with Kernel Planckster to request the extraction of relevant data from the internet or to download stored data through the Lieutenant Planckster platform on Kubeflow.

For more information on Kernel Planckster and how to use Kernel Planckster REST API, refer to the [Kernel Planckster Guides](../category/kernel-planckster)

## Data Scrapers

A Data Scraper is a program that automatically extracts relevant data from websites. There are three scrapers that are already set up in the Planckster Ecosystem: a Telegram scraper, a Twitter scraper and a Sentinel scraper. These scrapers retrive information from its corresponding website and then send the information to the Kernel Planckster software, which registers it in the Metadata database and saves it in the Block Storage element. The scraper can also augment the data using AI and LLM algorithms before sending it to Kernel Planckster.

For more information on how the data scrapers work, refer to the [Data Scrapers core concept document](./scrapers.md)

## Lieutenant Planckster

Lieutenant Planckster is a pipeline builder established in Kubeflow Platform. Pipelines are where all the different components - including code and images stored remotely - are pulled together into a workflow. Lieutenant Planckster is like the control centre from which the user can interact or send orders to the Kernel Planckster. These orders are represented in code that is executed to put Kernel Planckster to work. 

For more information on Lieutenant Planckster, refer to the [Lieutenant Planckster Guides](../category/lieutenant-planckster)

## Webstat Planckster








