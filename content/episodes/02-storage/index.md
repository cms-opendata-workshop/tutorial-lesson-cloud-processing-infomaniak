+++
title = "Storage for the output files"
weight = 20
teaching = 15
exercises = 10
questions = [" How to create an object storage container on Infomaniak resources?", "What are the basic operations?", "What are the costs of storage and download?"]
objectives = ["Learn to create a persistent storage for the job output.", "Learn to list the contents of the storage.","Understand the persistant storage costs."]
keypoints = ["The storage cost depends on the volume stored and the duration of storage, and for this type of processing is very small."]
+++

The processing workflow writes the output files to a storage from which they can be downloaded afterwards. 

For this tutorial, we use [Infomaniak Object Storage](https://docs.infomaniak.cloud/object_storage/swift_object_storage/). 

{{< callout type="note" title="Persistent storage" >}}
Storage resources are separate from the cluster computing resources. You can delete the cluster just after the processing and avoid unnecessary costs, but keep the output files.
{{< /callout >}}



{{< callout type="prereq" title="Prerequisites" >}}
- [ ] You will need the Infomaniak account and a public cloud project. See the [Setup]({{< relref "/learners/setup/#infomaniak" >}}) instructions.
- [ ] You will need to have the tutorial code in your local working environment and the software needed to handle storage installed. See the [Setup]({{< relref "/learners/setup/#software-setup" >}}) instructions.
{{< /callout >}}

Move to your local working environment and get the access to the storarge by sourcing the OpenStack RC file that you have downloaded previously.
If you use a virtual environment, activate it.



```bash
cd <your_path>/infomaniak-cloud-test
source venv/bin/activate # if you created a virtual environemnt with name venv
source <pat_to_rc_file_if_not_local>/PCU-XXXXXX-openrc.txt
```


## Storage

### Create

Create the storage - if not already done - with

```bash
openstack container create mystorage
```

You can list the existing storage containers with

```bash
openstack container list
```

### Test

Before continuing, go through some basic operations for the storage container.

#### Write

Create a test file and write it into the storage

```bash
echo "Storage test" > test.txt
openstack object create mystorage test.txt
```

#### Inspect

You can list the contents with

```bash
$ openstack object list mystorage
+----------+
| Name     |
+----------+
| test.txt |
+----------+
```

or 

```bash
$ swift list mystorage
test.txt
```

You can also login to the Infomaniak [Horizon dashboard](https://api.pub2.infomaniak.cloud/horizon/), reachable from a top-right button of the Cloud Computing dashboard or from the cloud computing project view.

{{< callout type="warning" title="Dashboard login" >}}
The Horizon dashboard login name is the OpenStack ID that starts with `PCU`.
Do not confuse it with the project ID that starts with `PCP`.
{{< /callout >}}

{{< callout type="warning" title="Dashboard region" >}}
To see your resouces, you need to select the region that you have chosen when downloading the OpenStack RC file from the dashboard top bar. You can check the region name in the OpenStack RC file, or in your working environment with `echo $OS_REGION_NAME`. 
{{< /callout >}}

You will find the storage container by choosing "Object Store" from the left bar.

#### Download

The file is now stored on the Infomaniak object storage. You can access it anywhere provided you have sourced the OpenStack RC file and have the Python tools (`openstack`and `swift` installed).

You can download it with (to a different file with the `-o` option, omit it to keep the same name):

```bash
$ swift download mystorage test.txt -o downloaded.txt
test.txt [auth 1.021s, headers 1.428s, total 1.430s, 0.000 MB/s]
```

or write to stdout with

```bash
$ swift download mystorage test.txt -o -
Storage test
```

A download option is also available from the Horizon dasboard

#### Delete

You can delete the test file with

```bash
swift delete mystorage  test.txt
```
## Costs

{{< callout type="note" title="Pricing details" >}}
You can find the current prices in the [Infomaniak cloud services pricing details](https://www.infomaniak.com/en/hosting/public-cloud/prices).
{{< /callout >}}

### Storage

The cost for the storage is currently 0.000013 €/GB/hour, i.e. 0.11 €/GB/year,
114 €/TB/year.
In the context of this tutorial, the cost is very small, but keep it in mind if you plan to store large output files for a long time.


### Data access

Uploading to the storage is free.

Downloading is free for the 10 first TB per month and then 0.00811 €/GB i.e 8.11 €/TB. 

{{< callout type="note" title="In comparison" >}}
Free download is a big advantage for Infomaniak in comparison to other cloud service providers. For example, the output file download cost of the type of processing in this tutorial [using Google Cloud Storage](https://cms-opendata-workshop.github.io/tutorial-lesson-cloud-processing-gcp/02-storage.html#storage) can be as high as half of the computing costs.
{{< /callout >}}



