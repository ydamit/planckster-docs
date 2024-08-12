---
sidebar_position: 6
---

# Examples

Let's discover **Source Code Examples**.

## Kubeflow Notebooks

To view all available notebooks, visit the [Notebooks tab](https://kubeflow.devmaany.com/_/jupyter/?ns=planckster-example) in the Kubeflow UI. 
Once you have selected a notebook, you can view its details, and click on "Connect" to interact with the notebook whenever you're ready:

- The [Kernel Planckster example notebook](https://kubeflow.devmaany.com/jupyter/notebook/details/planckster-example/example-kernel-planckster) tos show to connect to Kernel Planckster and view the scraped data obtained when running the Kubeflow pipelines. For more information on Kernel Planckster's API, please refer to the [corresponding guide](./guides/kernel/kernel-planckster-api-walkthrough/index.md)

- The [Disaster tracking example notebook](https://kubeflow.devmaany.com/_/jupyter/notebook/details/planckster-example/sda-disaster-tracking-demos?ns=planckster-example) shows how to get the data and run cv plotting and sutff like that

- Additionally, the [Disaster scraper example notebook](https://kubeflow.devmaany.com/_/jupyter/notebook/details/planckster-example/sda-disaster-tracking-scrapers?ns=planckster-example) exemplify how to get the data from the sources. You can build your own scraper notebook to get data from a new source by following the same structure.


## Data Scrapers

We are providing a set of data scrapers that you can use to get data from different sources.
The source code is freely available on their corresponding repositories.
You can take these scrapers as a starting point to build your own scrapers to get data from new sources:

- The [Sentinel scraper](https://github.com/dream-aim-deliver/mpi-sda-sentinel) for Sentinel satellite data
- The [Twitter scraper](https://github.com/dream-aim-deliver/mpi-sda-twitter-scraper) for Twitter data
- The [Telegram scraper](https://github.com/dream-aim-deliver/mpi-sda-telegram-scraper) to scrape Telegram channels
- The [Augmentation scraper](https://github.com/dream-aim-deliver/mpi-sda-augmentation), that runs at the end of the other three scrapers, to augment the data


