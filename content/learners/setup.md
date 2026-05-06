+++
title = "Setup"
weight = 10
+++

## Infomaniak 


### Account and project

Create an Infomaniak account by following "Get started for free" from the [Infomaniak entry page](https://www.infomaniak.com/). 

{{< callout type="note" title="Free resources" >}}
Note that in addition to cloud computing resources, which we will use for this tutorial, Infomaniak also offers a [comprehensive set of collaborative tools](https://www.infomaniak.com/en/special-offers?filter=free) (including e-mail, cloud storage, and many other services) for free, permanently.
{{< /callout >}}


To start the [free trial for cloud resources](https://www.infomaniak.com/en/hosting/public-cloud) (300 EUR/CHF), you need to order a "public cloud". A credit card number (or Twint in Switzerland) is needed. You will not be charged during the free trial.

Choose "Cloud Computing" from the left bar and select "Public Cloud".

![A screenshot from the Cloud Computing dashboard initial page.](fig/InfomaniakPublicCloud.png)


Click on "Order a Public Cloud" and give it a name. In the following steps:
- a phone number is requested to confirm your identity.
- you will be shown a number to call for verification (the confirmation is automatic when you call).
- provide an address.
- skip the suggested back-up option.
- in the final step, a credit card is requested (there are no charges for an empty project).

Creating your "Public Cloud" might take a while. First, after some time after placing the order, a request for ID verification will appear on the [Administration console](https://manager.infomaniak.com/). 
You will be asked to install Infomaniak Check app on your phone. You will be shown a QR code which you can scan with the app. To identify, you will need to scan your passport or ID card, and take a selfie with that document. Then, once the verification is ready, your project will appear on the [Cloud Computing dashboard](https://manager.infomaniak.com/v3/ng/products/cloud/public-cloud).

![A screenshot from the Cloud Computing dashboard page with the listing.](fig/InfomaniakPublicCloudCreated.png)


{{< callout type="note" title="Trial period" >}}
Creating a "Public Cloud" will start your free trial period of three months.
No charges occur for a project itself, only for the resources that you will create within. The cost will be deducted from the free credit during the trial period.
{{< /callout >}}

Start a project following the [Infomaniak instructions](https://www.infomaniak.com/en/support/faq/2603/create-a-new-public-cloud-project).

{{< callout type="note" title="Products and projects" >}}
Note the two layers in the Infomaniak structure. There is the **product**, in our case the "Public Cloud", and, below it, the **project** that we just created. The cloud resources needed in this tutorial are created in the project.
{{< /callout >}}

The OpenStack password that you will be asked to define will be needed to access the storage in this tutorial. Do not forget it!

### OpenStack access

Once the Public Cloud project has been created, you will find it on your public cloud page. 

See the [Infomaniak instructions](https://www.infomaniak.com/en/support/faq/2604/manage-actions-on-an-existing-public-cloud-project) about managing your project, and download the OpenStackRC file to the working area on your computer.

![The option for the OpenStackRC file download is visible from the three dots of the OpenStack access listing of the project view.](fig/InfomaniakCloudOpenStackRC.png)

When prompted, keep the suggested region. The file is named as `PCU-XXXXXX-openrc.txt` where `PCU-XXXXXX` is the OpenStack ID of your project.


## Software setup

{{< callout type="warning" title="Important" >}}
In this tutorial, we expect that you will be using a **Linux shell**: on native Linux, macOS, or through WSL2 on Windows.
{{< /callout >}}


### Get the code

```bash
git clone git@github.com:katilp/infomaniak-cloud-test.git  # or: git clone https://github.com/katilp/infomaniak-cloud-test.git if no SSH key
cd infomaniak-cloud-test
```

### Interacting with storage: openstack and swift

{{< callout type="note" title="Virtual environment" >}}
If you prefer to avoid interference with your usual Python environment, create a virtual environment in your working directory and activate it:

```bash
python3 -m venv venv
source venv/bin/activate
```

`(venv)` in front your prompt indicates that this environment has been activated.
The exit command is `deactivate`.
{{< /callout >}}


Install the tools to interact with the storage

```bash
pip install python-openstackclient python-swiftclient
```

The `requirements.txt` file in the repository contains the Python packages that got installed with the command above and used for testing this lesson. They might be less recent than what you get.

To connect to the project, copy the downloaded `PCU-XXXXXX-openrc.txt` to your working directory and source it:

```bash
source PCU-XXXXXX-openrc.txt
```

It asks the OpenStack password, which you defined when creating the project.

We will be using object storage in this tutorial. You can check your storage and as it is still empty, you will see something similar to this:

```bash
 openstack object store account show
+------------+---------------------------------------+
| Field      | Value                                 |
+------------+---------------------------------------+
| Account    | AUTH_123c4567df4a496e890123456aae9876 |
| Bytes      | 0                                     |
| Containers | 0                                     |
| Objects    | 0                                     |
| properties | quota-bytes='1000000000000'           |
+------------+---------------------------------------+
```

You can create a storage container named `mystorage` with

```bash
openstack container create mystorage
```

and list the existing storage containers with

```bash
openstack container list
```

{{< callout type="note" title="Storage cost" >}}
There is a cost for the storage (currently 0.000013 €/GB/hour, i.e. 0.11 €/GB/year to save you the math), but not for an empty container so just creating a storage container does not consume the free credit.

You can find the current prices in the [Infomaniak cloud services pricing details](https://www.infomaniak.com/en/hosting/public-cloud/prices).
{{< /callout >}}


### Kubernetes command-line tool: kubectl

`kubectl` is the tool to interact with the Kubernetes clusters. Install it either [on its own](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management) or with [Docker Desktop](https://www.docker.com/products/docker-desktop/).

<!-- ### Deploying resources: Terraform

This tutorial uses Terraform scripts to facilitate the creation and deletion of GCP resources. Install it following [these instructions](https://developer.hashicorp.com/terraform/install) (WSL2 users should follow the Linux instructions). -->

### Running workflows: Argo CLI

The example processing workflow is defined as an "Argo workflow". To be able to submit it and follow its progress from your terminal, download Argo command-line interface following [these instructions](https://github.com/argoproj/argo-workflows/releases/): 

```bash
curl -sLO "https://github.com/argoproj/argo-workflows/releases/download/v4.0.1/argo-linux-amd64.gz"
gunzip "argo-linux-amd64.gz"
chmod +x "argo-linux-amd64"
sudo mv "./argo-linux-amd64" /usr/local/bin/argo # or without sudo to a path (in your PATH) where you can write 
```

Take note that what is under "Controller and Server" in the instructions will be done once the cluster is available, do not do it yet.

### Optional for building a customized CMSSW container image: Docker

This tutorial uses a CMSSW open data container image with the [pfnano producer code](https://opendata.cern.ch/record/12504) downloaded and compiled. You do not need to install Docker to use it in the context of this tutorial. You need Docker, if you want to modify the code with your selections and build a new container image. 


## Next step

Once done, read the [Introduction]({{< relref "/episodes/01-introduction" >}}), or continue directly to [Storage for the output files]({{< relref "/episodes/02-storage" >}}).