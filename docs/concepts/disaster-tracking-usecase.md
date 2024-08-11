---
sidebar_label: Disaster Tracking Usecase
sidebar_position: 4
---

# Description of Kubeflow Pipelines for the Disaster Tracking Usecase:

## Understanding the DAG structure:

The DAG or *Directed Acyclic Graph* is the visual representation of a Kubeflow pipeline. It consists of *pipeline nodes* which are actually *wrappers* that configure and run a docker container.

### Scraper DAG:
![Scraper DAG](images/scraper-dag.png)

The Scraper DAG consists of **3 nodes** which run in parallel. Each node is a wrapper for one of the three included scrapers (sentinel, telegram, twitter).

### Augmentation Engine DAG
![Augmentation  DAG](images/augmentation-dag.png)

The Augmentation Engine DAG consists of **1 node** representing the wrapper for the disaster-usecase augmentation engine. This pipeline is run **after** the scraper pipeline successfully completes its run (meaning that all three node complete their respective runs):

![A Successful Run](images/successful-scraper-run.png)

## Properties of a DAG element
![Sentinel node properties 1](images/sentinel-properties-1.png)
![Sentinel node properties 2](images/sentinel-properties-2.png)

A list of properties for an node (in this case sentinel) can be viewed by clicking on it. The most important properties include **Input parameters**, **Arguments**, **Image**, and **Version** (in the "Summary" box). 

### Input Parameters

Input parameters are common to the *entire pipeline* and thus every single node in the scraper pipeline shares the same start_date, end_date, job_id, and tracer_id. 

It is **essential** that these input parameters are shared across scrapers, especially job_id and tracer_id. This is because all scrapers upload their files to MinIO using the *relative-path* "/SCRAPER/TRACER_ID/JOB_ID/" where SCRAPER is one of "twitter", "telegram", or "sentinel". 

The disaster-tracking augmentation engine *expects* all scrapers to upload their *final augmented files* to "sda/SCRAPER/TRACER_ID/JOB_ID/augmented". The augmentation engine downloads all files from this path to a temporary directory, runs a matching-algorithm, and uploads the final matched files to MinIO using the *relative-path* "/augmented/TRACER_ID/JOB_ID/by_date/". 

Thus, runs for both the scraper-pipeline and augmentation-pipeline need to be constructed using the same job_id and tracer_id. This simple action allows for **linking two pipelines** which is a vital part of our overall workflow. 

### Arguments

Arguments are what are fed via the CMD line to the scraper before it runs. Unlike Input parameters, arguments are *node-specific* as each scraper uses different arguments depending on its context and function. Note that start-date, end_date, job_id, and tracer_id are actually *arguments*; however, their values are inherited from their *Input Parameter Value*. Note that the scraper itself actually has more arguments like api keys; however, many of these arguments are hardcoded in and thus we can only see arguments that are available to the wrapper/node. 

* The sentinel-specific arguments are "long-left" (leftmost longitude), "lat-down" (down-most latitude), "long-right" (rightmost longitude), "lat-up" (topmost latitude), and "resolution" (height of image in meters) which together represent the geographic bounding box and level of detail for a region. 
* The only telegram-specific argument is "channel-name" which is the specific publicly-available channel that should be scraped. 
* The only twitter-specific argument is "query" which is the unique search-term to use when scraping tweets.
* The augmentation engine takes no additional arguments. 

### Image

The Image name is critical as it specifies which dockerized representation of the scraper the wrapper should use. For sentinel, we choose the image "maany/mpi-sda-satellite:2.0.0" hosted on DockerHub (the other scrapers and augmentation engine are named similarly). 

Note: we have setup release automation on our github for each of the scraper/augmentation repositories which automatically create a docker image for each major code release.

### Version

The version name of the pipeline is important as every time the docker image for an node is updated to a new version the pipeline should be recompiled. To avoid conflicts, this recompiled pipeline should have an updated version name as well. 

![Augmentation pipeline properties](images/augmentation-properties.png)

For example the Augmentation pipeline uses an updated container image with version "2.1.0", so the pipeline version is now named "augmentation-pipeline-2.1".

## Pipeline Notebooks

#### Each Pipeline/DAG is compiled from a notebook. 

* The scraper-pipeline is compiled from the "sda-disaster-tracking-scrapers" notebook. 
* The augmentation-pipeline is compiled from the "sda-disaster-tracking-augmentation" notebook. 

### Wrappers

The notebook for a pipeline defines all of the nodes and their properties via *wrappers*. 

![Sentinel Wrapper](images/sentinel-wrapper.png)

Note how the scraper itself has more CMD line arguments than what it exposes to its wrapper/node. This makes pipelines easier to configure *on a usecase basis* by focusing on real-world arguments like dates and coordinates rather than code-specific arguments like API keys. 


### Setup the pipeline/DAG

The order nodes run in is determined by the how the pipeline is setup:

![Scrapers task](images/scrapers-task.png)

Because all three scraper tasks are executed in a single function, they run in parallel and the pipeline has a DAG with three parallel elements (as seen previously.)

### Compile the pipeline/DAG

In order to view the pipeline and have the ability to create a run, it must be compiled and uploaded with a unique version name:

![Compile Pipeline](images/compile-pipeline.png)

You can now navigate to the *pipelines* section of Kubeflow and create a run for the disaster tracking usecase. Once again, make sure to run the scraper pipeline *first*, check logs and MinIO to make sure the "/SCRAPER/TRACER_ID/JOB_ID/augmented" is populated for every scraper. Then run the augmentation pipeline and ensure "/augmented/TRACER_ID/JOB_ID/by_date" is populated with matched data. To work with this data, open the "SDA-disaster-tracking-demos" notebook or view the "analyzing-scraped-data" guide. 


### Conclusion

You now understand how the disaster usecase is constructed and how augmentation works. To learn more about how wrappers work and how you can build your own custom pipelines, see the guide titled "custom-pipelines". To work with the data produced by the scrapers and the augmentation engine, open the "SDA-disaster-tracking-demos" notebook or view the "analyzing-scraped-data" guide. 