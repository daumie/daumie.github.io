---
title: "Associate Cloud Engineer Notes"
date: 2019-10-20T23:37:41+03:00
draft: false
toc: false
images:
tags: 
  - untagged
---

## Short Notes In Preparation of Google Associate Cloud Engineer Exam

### Stackdriver ( Monitoring, logging, and debugging. )

Stackdriver metadata is stored in the `host` project.

Stackdriver was previously a third-party application. When using Stackdriver, you may be transferred between different domains.

A good project ID describes the project and environment e.g `bench-projects-production`

When creating a new account, choose the `payments profile` to pay for a Google Play developer license.

When adding users from **Active Directory** you will need `Google Cloud Directory Sync`

Google Cloud requests payment information for all accounts to make sure the user is not a bot.

A *project id* is a globally-unique identifier used by engineers that we can set ourselves.

N/B
> A project ID is different from a project name. The project name is a human-readable way to identify your projects, but it isn't used by any Google APIs.

`Individual` google cloud account is for personal use on GCP.

Stack driver best Practice:

+ If monitoring one project, use that project as the host project.
+ If monitoring multiple projects, create an empty project for the host project.

### Managing Billing Information

When using file export, the JSON or CSV file is saved *Daily* to the Cloud Storage Bucket.

`File export` is the best option for importing a CSV file into an Excel spreadsheet.

`BigQuery export` runs throughout the day and streams spending data into the dataset as it is received

`Billing Account User` role allows users to link and unlink projects. It is the best available option for someone who will be creating billable projects.

`Billing Account Admin` role is well suited for users who need to link projects and manage user roles and payment details. It is considered an owner role for the billing account. The legacy Owner role is a primitive role. It is not recommended due to its broad permissions. Users should be granted the least permissive access necessary to do their job.

`Invoiced billing account:` is

+ Billed monthly
+ Can be paid for via check or wire transfer
+ Must be set up through the Google Cloud sales team.

BigQuery is a service that is used to analyze big datasets. Due to the size of the data, _BigQuery export_ is the best option for analyzing spending across multiple projects.

### Cloud SDK

The `core component` is a shared library between different components.

The `gcloud init command` is used to configure and initialize the Cloud SDK. It is a shortcut for _login_ and _config_ commands.

The `gcloud alpha` and `gcloud beta` commands are two groups of additional Cloud SDK commands that you can install for the gcloud component.

If you use a package manager to install Cloud SDK, the same package manager must be used to install additional components. For a list of components and corresponding apt or yum packages, go [here](https://cloud.google.com/sdk/docs/components#additional_components)

To view which project is default, run `gcloud config list` command. This will list properties for the active configuration, including the default project.

### Planning and Configuring a Cloud Solution

`gcloud compute images list` list all the images and their family names.

A static website can be hosted with `cloud storage` for very little money.

A `service` is how we expose `deployments`. It's a persistent endpoint that we can interact with, and it will send the traffic over to the pods in the deployment.

`Binary logging` in CLoud SQL allows you to use point-in-time recovery.

App Engine; Flexible environments are able to use a Dockerfile to create custom runtimes. They specifically run on port 8080.

Before connecting into a Windows instance you need to have a password generated. Then you can use any RDP client you want.

An autoscaled managed instance group will make it easy to have instances added automatically. Using a preemptible instance will save cost.

`Network TCP/UDP` supports regional load balancing and preserves client IP addresses.

Kubernetes gives each pod its own IP address. Services allow pods to communicate without using their individual IP address.

`SSL Proxy` is the appropriate load balancer for TCP traffic with SSL offload. It supports global, external load balancing.

Compute Engine is optimal in:

+ Workloads requiring high performance
+ Workloads that will use preemptible instances
+ Workloads requiring control of the operating system

`Cloud Spanner` is a modern SQL database, designed for the cloud. Its managed instances are always running, and it offers horizontal scaling.

Limitations of preemptible instances:

+ They are terminated after 24 hours.
+ They can be terminated at any time if that capacity is needed.

Compute Engine requires the most operational effort. It allows us to have OS-level control.

A major advantage of using the `App Engine flexible` environment is the ability to customize the runtime. ( Using Dockerfiles )

`DaemonSet controller` ensures that a copy of a pod runs on nodes in the cluster, allowing for node management.

`Machine types` define the hardware configuration concerning core, memory, and GPUs. There are machine types that are predefined by Google, or you can designate a custom machine type for more control.

When using an auto mode network, Google automatically creates subnets in each region using distinct, predefined IP ranges.

The `App Engine standard environment` does not support custom runtimes. We cannot install third-party binaries or write to disk with older runtime versions.

Managed instance groups

+ Additional instances are removed 10 minutes after they are no longer required.
+ They offer high availability, autoscaling, and automatic updates.

`Cloud Bigtable` is designed for massive amounts of data, as in terabytes.

### Deploying and Implementing a Cloud Solution

Set the `Content-Type metadata` for an object in cloud storage to "application/pdf" will ensure the browser views the object as a PDF.

Change memory of an instance: Stop the instance and change the machine type.

`gcloud deployment-manager deployments update` updates an existing deployment.

Setting `allUsers` to have the Storage _Object Viewer_ role. makes a bucket public.

Using the `gsutil cors set` command sets the _CORS configuration_ on a given bucket.

When creating the instance template using a startup script is a simple way to get an application bootstrapped without adding more tooling.

Marketplace, formally Cloud Launcher, uses `Deployment Manager` on the back-end.

`Service accounts` make it easier for software to interact with various Google services. Service accounts are designed to be used by services (such as Compute Engine) and code instead of people.

Kubernetes resources make use of `YAML` files to configure deployments.

Benefits of using Deployment Manager:

+ Deployment Manager makes REST API calls for us.
+ Deployment Manager helps us reproduce projects and reuse code.
+ We can create templates in Jinja or Python.

Run `gcloud container clusters get-credentials` to authenticate and configure `kubectl`

The `gcloud functions deploy` command is used to create, deploy, and update Cloud Functions.

Access scopes are a legacy predating Cloud IAM. They are used to assign permissions to VM instances.

Call `gsutil iam ch` to change a private storage bucket to public.

To make a storage bucket public, set the `allUsers` group to have `objectViewer` rights.

Running `kubectl create secret generic service-account-file` combines `kubectl create secret` and `kubectl update secret`.

Cloud Functions be triggered by `Cloud Pub/Sub` service.

According to Application Default Credentials (ADC), a Google Cloud client library locates application credentials;

+ The first step is for ADC to check if environment variable GOOGLE_APPLICATION_CREDENTIALS is set to the service account key.

`gsutil mb` (mb = make bucket) is used to create a storage bucket for Cloud Storage.

Setup scripts such as service account and Kubernetes cluster should run once during the initial setup.

`CIDR` notation allows us to create an IP address range. It stands for Classless Inter-domain Routing.

**URL** handlers are evaluated in the order that they are listed in the app.yaml file. Therefore, they are evaluated from top to bottom.

`gcloud deployment-manager deployments update` updates an existing deployment.

### Managing Resources and Working with Logs

`gsutil ls`  command is used to show a list of Cloud Storage buckets

A `snapshot` is a point-in-time copy of a persistent disk.

`gsutil signurl -d 3m` ( The object can be viewed for a duration of 3 minutes)

Log sinks can export records to `Cloud Pub/Sub`

We can split traffic in App Engine by:

+ IP address
+ By cookie
+ Random

`gcloud config configurations activate` activates an existing configuration.

`App Engine` ; If an application has more than one version, the default behavior is to route 100% of traffic to the newer version.

`gcloud config list` list the settings for the active configuration.

`Basic scaling` can NOT be used by flexible environments.

The following can be used to create a new image:

+ Snapshot
+ Cloud Storage file
+ Disk
+ Image

Not all GPU types are available in every zone.

In order to _delete pods_ within a deployment, you will need to _delete the deployment_.

CIDR notation - The smaller the mask bit, the higher the number of IP addresses in the range.

Documentation in an alert policy is the content of the message that is sent with the notification.

### Ensure successful operation of a cloud solution

Each log sink destination has its own time window for saving the data.

Object management - Create a lifecycle policy to switch the objects older than a given "time" to nearline/coldline (desired) storage.

### Configure access and security

`Flow logs` are great for gaining insights into what's happening on a network. They provide a sample of the flows to and from instances.

`gcloud iam roles copy` is a fast way to copy existing roles, even across projects.

Using composite objects and parallel uploads to upload the file to Cloud Storage quickly will allow you to upload the file quickly by breaking it into smaller chunks and uploading them at the same time.

_Exposed secret on git_ --- Revoke the key, remove the key from Git, purge the Git history to remove all traces of the file, ensure the key is added to the .gitignore file.

(`allUsers` and a`llAuthenticatedUsers`) Either of these tokens represents public users. allAuthenticatedUsers represents a user with a Google account. They don't need to be part of your organization. Neither token should be used to grant permissions unless the bucket is truly public.

The API Explorer is likely the fastest and easiest way to test out an API. It doesn't require all of the additional effort that goes along with setting up APIs for production environments.

If you use a _package manager_ (Apt or Yum) to install the `Cloud SDK`, then you can't use the built-in component manager to install or remove components.
