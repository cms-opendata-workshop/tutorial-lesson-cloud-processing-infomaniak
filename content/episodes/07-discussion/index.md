+++
title = "Discussion"
weight = 70
teaching = 15
exercises = 0
questions = ["What was the user experience?", "What worked well?", "What difficulties were observed?"]
objectives = ["Learn about the practical user experience with CMS open data processing on Infomaniak resources."]
keypoints = ["Technically, deploying the resources, setting up the workflow and running the processing on Infomaniak was very smooth.", "The Infomaniak support team reacted within the expected delay.", "Considering the ease of setup and very advantegous pricing, Infomaniak can be a very convenient choice for small-scale processing by individual researchers."]
+++

## What worked?

Setting up resources was straight-forward. The documentation is adequate, very good for starting a project and for storage resources, but a bit less elaborated for Kubenetes services. However, this was not found to be an issue as the setup is standard Kubernetes without provider-specific add-ons.

Various Kubernetes roles and bindings to handle access between computing and storage resources were not needed, which simplifies the cluster setup. Access to the storage is handled with standard OpenStack credentials and they can be smoothly used in the Argo workflow definitions.

The initial quota for different resources during the free trial period was large enough for full-scale processing on CPU nodes. When enquired, the support team promptly increased them, but finally none of these extra resources were needed in this type of processing.

The Infomaniak support team replied to questions within expected delays whenever help was needed.


## What difficulties were observed?

Two useful functionalities are currently missing. 

{{% steps %}}

### Image disk
An option to start the cluster nodes with a preprepared image disk containing the large CMSSW container image would be very helpful to speed up the job start. It would also avoid repeated image pulls and recude the risk of hitting the pull limits of image registries. This is not yet available.

### kubectl top
To inspect the resource usage of the processing jobs, `kubectl top` commands would offer a handy command-line option. The metrics API needed for it is, however, not available.
{{% /steps %}}


