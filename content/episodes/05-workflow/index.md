+++
title = "Setup a workflow"
weight = 50
teaching = 15
exercises = 30
questions = ["How to set up Argo Workflow engine?", "How to submit a test job?","Where to find the output?"]
objectives = ["Deploy Argo Workflows services to the cluster.","Submit a test job.","Find the output in the storage."]
keypoints = ["Once the cluster is up, you will first deploy the Argo Workflows services using kubectl.","You will submit and monitor the workflow with argo."]
+++

{{< callout type="prereq" title="Prerequisites" >}}
- You will need the Infomaniak account and a public cloud project. See the [Setup]({{< relref "/learners/setup/#infomaniak" >}}) instructions.
- You will need to have 
  - the tutorial code in your local working environment 
  - `kubectl` installed to interact with Kubernetes cluster
  - `openstack` and `swift` installed to handle storage
  - `argo` CLI to submit the jobs installed.
  - See the [Setup]({{< relref "/learners/setup/#software-setup" >}}) instructions.
- You wil need the storage container and cluster created as instructed on the previous pages.
{{< /callout >}}

## Deploy Argo Workflows service

