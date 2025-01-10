---
sidebar_label: Introduction
sidebar_position: 1
---

# Lieutenant Planckster Instructional Video

[Click here for a quick video guide](https://youtu.be/arP1y2DqECw) for using Lt. Planckster! [This video](https://youtu.be/arP1y2DqECw) provides the most basic instructions for how to use Kubeflow in the context of Kernel Planckster. It may be sufficient on its own, but can be combined with the documentation provided in this section for a deeper understanding of the data processing machinery that supports Case Studies in Websat.

# Kubeflow

Kubeflow is a platform that makes it easier to house, test, automate, and monitor the machinery necessary for AI / Machine Learning based workflows. It's a kind of "command center" that allows you to orchestrate the code, data sources, and other resources you need to use.

Kubeflow is a Creative Commons project, whose components are based on the Open Source [Kubernetes](https://kubernetes.io/) project. You can learn more about Kubeflow on [their main website](https://www.kubeflow.org/).

In the context of [Kernel Planckster](/docs/category/kernel-planckster), Kubeflow's primary function is to manage the scraping [Pipelines](#pipelines) that feed materials into Research Contexts. In other words, Kubeflow receives data ("intelligence") and broad directives ("orders") from Kernel Planckster, and puts together all the pieces necessary to execute them - hence the name "Lieutenant Planckster". But Kubeflow can also be used to test and troubleshoot these Pipelines, interact _ad hoc_ with the data as needed, and more.

<details>
	<summary>UI Tips</summary>

    All of the list pages described below have some useful searching and filtering options.

    **Filtering**: List pages can be filtered by typing a search term into the Filter bar at the top. There are two types of filters, though.

    1. Static filters have no border, the prompt doesn't name the type of object being filtered, but provides options for filter category, and has a question mark tooltip at the far right with info on advanced options. To apply this sort of filter, it's necessary to submit (e.g. by pressing the return button) after typing the filter term. ![screen shot of static filter of "disaster" applied on Notebooks page](/img/kubeflow_filter_static.png)
    1. Dynamic filters have a border, name the type of object being filtered (e.g. "filter runs"), but provide no other options and no tooltip. These filters begin applying as you type. ![screen shot of dynamic filter of "augmen" applied on Runs page](/img/kubeflow_filter_dynamic.png)

    **Pagination**: By default, most list pages show 10 items per page. Pagination can be found at the bottom-right of the page, but it's subtle and can be easy to miss! The pagination allows you to move through pages, but also to change how many items show per page. ![screen shot of pagination showing the options for how many items to show per page](/img/kubeflow_pagination.png)
</details>

## Notebooks

Notebooks are where you write _ad hoc_ code to interact with all the remote resources that make up or are produced by the [Pipelines](#pipelines) you run on Kubeflow. You can use them to load environments and images, call code you have saved remotely, interact with data, files, or other resources that are the inputs or outputs to your workflows, and more. Each Notebook is actually a whole project on its own server, which can contain any number of files - including Jupyter Notebooks (.ipynb) - for you to interact with or output.

<details>
	<summary>Notebooks List View</summary>

	On the [Notebooks page](https://kubeflow.devmaany.com/_/jupyter/?ns=planckster-example), you can see all of these projects listed.

	![screen shot of Notebooks list](/img/kubeflow_notebooks_list.png)
</details>

<details>
	<summary>Notebook Detail View</summary>

	Clicking on one of the Pod Names shows details about its configuration, including applied [Volumes](https://kubeflow.devmaany.com/_/volumes/?ns=planckster-example), Environment, but also the resources allocated, as well as its history (condition, events, logs).

	![screen shot of Notebook detail showing allocated resources, environment, and some conditions](/img/kubeflow_notebook_detail.png)
</details>

To interact with the files in one of these projects, click CONNECT, which is a button at the top of the detail view, or a link on each item towards the right of the list view.

<details>
	<summary>Create New Notebook</summary>

	You can create a new Notebook by clicking the New Notebook button on the list view. From there, you will need to name the Notebook project, choose the type, allocate resources, create a new Workspace Volume, and either create a new Data Volume, or assign an existing one.

	![screen capture of assigning an existing Data Volume when creating a Notebook](/img/kubeflow_notebook_create.gif)

	**Note**: If the Notebook is to be used for creating Pipelines, you must explicitly "Allow access to Kubeflow Pipelines" in the configuration section's Advanced Options:

	![screen capture of allowing access to Kubeflow Pipelines when creating a Notebook](/img/kubeflow_notebook_create_access.gif)
</details>

## Pipelines

Pipelines are where all the different [components](https://www.kubeflow.org/docs/components/pipelines/concepts/component/) - including code and images stored remotely, such as the three scrapers - are pulled together into a workflow. In Lieutenant Planckster, these Pipelines are configured using YAML files that are created within corresponding [Notebooks](#Notebooks).

<details>
	<summary>Pipelines List View</summary>

	The [Pipelines page](https://kubeflow.devmaany.com/_/pipeline/?ns=planckster-example#/pipelines) shows a list of all the Pipelines that have been set up. You can click on the little triangle to the left of any of them to see the various versions that have been saved.

	![screen shot of Pipelines list with sda-disaster-tracking-scrapers open to show three versions](/img/kubeflow_pipelines_list.png)
</details>

<details>
	<summary>Pipeline Detail View</summary>

	You can click on the name of any of the versions to see both graph and markup (YAML) representations of that Pipeline (clicking on the overall Pipeline name takes you to the current, most recent version). In sda-disaster-tracking-scrapers, for example, you can see that the structure of the Pipeline is relatively simple: all three scrapers run in parallel, with no need for any complicated conditional sequencing, or any data to be passed between the Pipeline's components. But all of these layers are possible should the need arise!

	![screen shot of sda-disaster-tracking-scrapers Pipelines detail showing three parallel scrapers for Sentinel, Telegram, and Twitter](/img/kubeflow_pipeline_detail.png)

	You can manually run your Pipeline by clicking the Create Run button at the top. Note that you will need to specify an [Experiment](#Experiments) the run is associated with.
</details>

Pipelines are set up to run via [Experiments](#Experiments).

## Experiments

Experiments in Kubeflow [Pipelines](#Pipelines) are how Pipelines are run. In addition to production runs, they allow for test runs with controlled configurations and parameters. In the context of Lieutenant Planckster, Experiments are how the X (Twitter), Sentinel, and Telegram scrapers are regularly run in the background, without requiring any kind of manual trigger.

We can also use Experiments to troubleshoot Pipelines and ensure they perform as expected under various anticipated scenarios. Experiment [Runs](https://kubeflow.devmaany.com/_/pipeline/?ns=planckster-example#/runs) show details about the success or failure of the overall workflow, as well as individual components, so that Pipeline stability can be ensured before full deployment. Runs can be _ad hoc_ or set up to be recurring.

<details>
	<summary>Experiments List View</summary>

	The [Experiments page](https://kubeflow.devmaany.com/_/pipeline/?ns=planckster-example#/experiments) shows a list of all the Experiments that have been created. You can click on the little triangle to the left of any of them to see the performance of that Experiment's most recent runs.

	![screen shot of Experiments list with sda-disaster-tracking open to show outcomes of all five runs](/img/kubeflow_experiments_list.png)

	Clicking on the Run name takes you to the details of the run. The Config tab shows the parameters with which the Experiment was conducted.

	![screen shot of sda-disaster-tracking Experiment run detail showing run details and parameters](/img/kubeflow_experiment_run_details.png)

	Clicking on the Pipeline version takes you to the Pipeline Detail View.
</details>

The Experiment Detail View is effectively the same as what is revealed by clicking on the triangle next to an Experiment in the list view.