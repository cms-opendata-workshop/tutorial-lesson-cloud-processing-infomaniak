+++
title = "Introduction"
weight = 10
teaching = 10
exercises = 10
questions = ["What are public cloud providers?", "Why would you use them?", "What is Kubernetes?", "What else do you need?"]
objectives = ["Understand the motivation for using a Kubernetes cluster from public cloud providers.", "Learn the tools used to set up the Kubernetes cluster and run the processing workflow."]
keypoints = ["Public cloud providers are companies that offer pay-as-you-go computing resources and services over the internet to multiple users or organizations.", "Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications and their associated workflows across clusters of host machines.", "Argo Workflows is an open-source tool for orchestrating sequential and parallel jobs on Kubernetes."]
+++

This tutorial shows how to set up a CMS open data processing workflow using Kubernetes clusters from Infomaniak.

{{< callout type="note" title="Note" >}}
To learn about CMS open data and the different data formats, work through the tutorials in one of our [workshops](https://cms-opendata-guide.web.cern.ch/cmsOpenData/workshops/).
{{< /callout >}}

Using public cloud resources is an option for you if you do not have enough computing resources and want to run some heavy processing. In this tutorial, we use as an [example processing](https://opendata.cern.ch/record/12504) of CMS open data MiniAOD to a “custom” NanoAOD, including more information than the standard NanoAOD but still in the same flat file format.

We assume that you would want to download the output files to your local area and analyse them with your own resources. Note that analysis using Infomaniak resources with your files stored on Infomaniak storage is also possible, but is not covered in this tutorial.

## Infomaniak

Public cloud providers are companies that offers computing resources and services over the internet to multiple users or organizations. [Infomaniak](https://www.infomaniak.com/en/hosting/public-cloud) is one of them: it is a Swiss company, less known than its big competitors. You define and deploy the resources that you need and pay for what you use. As many other such resource providers (for example Google Cloud Platform, AWS, Azure, OHV), it offers some free getting-started “credits”.

{{< callout type="note" title="Note" >}}
Infomaniak offers free trial credits for 300 CHF for a new account. This credit is valid for 90 days.
{{< /callout >}}

{{< callout type="note" title="Note" >}}
Infomaniak resources are also considerably less expensive than those of the other providers, as you will note if you compare the observed cost of this same example [using Google Cloud Platform](https://cms-opendata-workshop.github.io/tutorial-lesson-cloud-processing-gcp/index.html). 
{{< /callout >}}


You can create, manage and delete resources using the [Infomaniak administrator console](https://manager.infomaniak.com/) (a Web UI). Note that an account is needed to access this page.

In this tutorial, we use `openstack` commands to create the persistent storage for the output data, `swift` to inspect and download them, and Infomaniak Web UI to provision the Kubernetes cluster where the processing workflow will run.

## OpenStack Swift

[OpenStack](https://www.openstack.org/) is a set of software components that provide common services for cloud infrastructure. In this tutorial, we its [Swift](https://docs.openstack.org/swift/latest/) component to interact with the object storage.

## Kubernetes

Kubernetes is a system to manage containerized workflows on computing clusters. `kubectl` is the command-line tool to interact with the cluster resources.

In this tutorial, we use `kubectl` commands to set up some services and to observe the status of the cluster and the data processing workflow. 

## Argo Workflows

The processing workflow consist of some sequential and parallel steps. We use [Argo Workflows](https://argoproj.github.io/workflows/) to define (or "orchestrate") the workflow steps.

In this tutorial, the Argo Workflows services are set up using a `kubectl` command. We use `argo`, the command-line tool, to submit and manage the workflows.

## Setup

If not yet done, prepare your environment as instructed in [Setup]({{< relref "/learners/setup" >}}).


