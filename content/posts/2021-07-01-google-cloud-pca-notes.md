---
title: "Google Cloud PCA"
date: 2021-07-01T12:10:05+00:00
slug: "google-cloud-pca-notes"
draft: false
description: "I recently passed my Google Cloud Professional Cloud Architect. I have consolidated some of the most useful information I found while studying and provide some analysis for the new case studies."
cover:
  image: "/images/2021/07/pca.png"
  relative: false
  hiddenInList: false
tags: ["Google", "Certificate", "PCA", "Architect", "cloud", "Professional Cloud Architect", "case studies", "new"]
---

I recently took the PCA and passed! It was tough but I thought it was worth it as I am better able to understand the context for Google Cloud much better.

---

Everyone tells you to read the docs and that they are the most useful thing when it comes to studying for the PCA.

Unfortunately this is like telling someone who is studying the English language to study the dictionary. It is useful as a reference, only in context of the questions you have! If you read it from end to end, it's just not a good use of your time!

In this post I break down how you can prepare for the PCA in the most efficient means possible.

The key idea when preparing for the PCA is to look out for content that discusses how things are designed and how various services in GCP can be used to fit that design criteria.

Google released their refreshed PCA exam on May 1. This means that the format and content will be shifted and it will take some time for content creators to keep up. I don't recommend using these resources as they can be quite variable in quality.

# The Experience

I took and passed the refreshed version of Google's PCA exam earlier this week. It is fairly challenging as the questions were long, often spanning a paragraph or so, and the answers were very similar.

# Some intuition

- If you need to do IAM, always add to groups
- If you want to prevent data exfiltration, look at VPC controls
- For sensitive information, run it through DLP first
- Understand what the different parts of Cloud Monitoring (formerly Stackdriver) does. Trace, Debugger, Monitoring, Error Reporting... etc

# Content from Google

- Google creates some very good content on their Youtube channel. I recommend you check it out.

2. Reading about the best practices for services is the way to go when trying to grasp what is the best thing to do, especially since when it comes to the questions, you are often asked to gauge what is the best/cheapest/fastest option.
{{< bookmark
  url="https://cloud.google.com/kubernetes-engine/docs/best-practices"
  title="Best practices | Kubernetes Engine Documentation | Google Cloud"
  favicon="https://www.gstatic.com/devrel-devsite/prod/v702c60b70d68da067f4d656556a48e4ab1cf14be10bb79e46f353f3fdfe8505d/cloud/images/favicons/onecloud/super_cloud.png"
  site="Google Cloud"
>}}
3. For general guidance, the best practices documentation for organizations, written by Google experts, is hard to beat.
{{< bookmark
  url="https://cloud.google.com/docs/enterprise/best-practices-for-enterprise-organizations"
  title="Best practices for enterprise organizations | Documentation"
  favicon="https://www.gstatic.com/devrel-devsite/prod/v702c60b70d68da067f4d656556a48e4ab1cf14be10bb79e46f353f3fdfe8505d/cloud/images/favicons/onecloud/super_cloud.png"
  site="Google Cloud"
>}}
4. The architecture framework gives you a super broad overview of GCP services and the context they can be used in.
{{< bookmark
  url="https://cloud.google.com/architecture/framework"
  title="Overview | Architecture Framework | Google Cloud"
  description="Google Cloud's architecture framework provices best practices for designing on Google Cloud."
  favicon="https://www.gstatic.com/devrel-devsite/prod/v702c60b70d68da067f4d656556a48e4ab1cf14be10bb79e46f353f3fdfe8505d/cloud/images/favicons/onecloud/super_cloud.png"
  site="Google Cloud"
>}}
# Other

I found that the courses provided on Coursera were not useful for my learning because it doesn't go into the details, only providing a generic overview of the services that GCP provides instead.

---

# Case Studies

I decided to do a detailed breakdown of the case studies

# Case Study: TerramEarth

The exam was updated on May 1st.

Over 500 dealers and service centers in 100 countries which means that they are a global company that needs solutions deployed globally.

Each vehicle needs 200 to 500 megabytes a day -> 2 mil vehicles - 20% yoy growth - this means that they cannot use BIgQuery directly, use Pub/Sub to ingest the data

This means a data consumption of 400TB to 1000TB - the only thing that can do this is BigQuery or BigTable.

## Business Considerations

### Prediction and detections for just in time repair

- Machine learning for predictions

### Decrease cloud operational costs and adapt to seasonality

A big way to control operational costs is through the use of preemptible machines and autoscaling infrastructure avalible in GCP to provide you the processing power when you need it.

- Google Kubernetes Engine provides autoscaling for nodes to scale up and down nodes if required. Do note that it will not scale down if it detects processing is > 50%. You can take this one step further and use an auto-managed infrastructure where only the Pods are scaled up or down.
- For small scripts and streaming data, a combination of Google Cloud Functions and Pub/Sub also charges based on the number of invokations
- For custom containers, Cloud Run provides the platform to run containers and scales them down to zero unless a `min-instance` value is set
- App Engine Standard scales down to zero by default, but note that App Engine Flexible does not scale down to zero - there's always at least one running.

### Increase speed and reliability of development workflow

- Deploy Jenkins as a managed solution from the Marketplace for a unified CI/CD pipeline to increase the delivery speed of the project.

### Remote developers to be productive without compromising code or data security

Here, remote most likely refers to the processes required for developers to develop while on an untrusted network or device without undue hinderance.

- BeyondCorp Enterprise provides secure access to corporate resources and workers and is specifically designed with zero trust security. This is closely tied to Identity Aware Proxy. All requests will come in through Cloud IAP to be authenticated and authorized. If it is an on-prem service, it will then pass through a firewall through an Cloud IAP connector to Interconnect and finally to the app.

### Flexible and scalable platform for developers to create custom API service for dealers and partners

- Cloud Endpoints - Given that this only supports a small subset of services on App Engine (Python 2.7 / Java), I highly doubt that this is an appropriate option
- Apigee - fully fledged API gateway, with a customizable portal for on-boarding partners and developers.
- API Gateway - Secure and monitor APIs for serverless backends

Of these options Apigee sounds like the most appropriate option because it is a more customer-facing Api management gateway whereas API gateway is targeted towards backend development

## Technical requirements

### New abstraction layer for HTTP API access to legacy systems

- Since API is mentioned and it seems to be mostly internal, Google Cloud Endpoints could serve as a potential solution as it allows us to connect to on-prem systems as well.
- API Gateway is not suitable here as it is meant for serverless backends

### Modern CI/CD pipelines to allow developers to deploy container based workloads in highly scalable environments

- Spinnaker's continuous delivery platform helps to manage clusters and deployments.
- GitOps methology ensures that all changes to applications are stored in source repos and are always accessible.
- Different deployment patterns such as: rolling update deployment, blue-green deployment, and canary deployments
- Container based workloads with highly scalable environments point towards GKE with the autoscaling option.

### Allow developers to run experiences without compromising security and governance requirements

- One of Google's own guidelines is to create separate projects for development and production environments.
- For sensitive data like credit cards, it is highly recommended that all processes that interact with the data be run within a separate project.
- Separation of environments and clusters for GKE between production and development.

### Create a self service portal for internal and partner developers to create new projects, request analytics jobs and manage access to API endpoints

- This matches the Apigee description to a T.

### Cloud-native solutions for keys and secrets management

- Google Cloud provides Secret Manager to store keys, passwords and certificates
- Customer Supplied Encryption Keys (CSEK - 100% liable, only available for Cloud Storage and Compute) are probably not cloud-native, and Customer Managed Encryption Keys (CMEK - you and google share management) are probably not too.

### Improve and standardize tools necessary for application and network monitoring and troubleshooting

- Google's Operations Suite (formerly Stackdriver) is key. It provides Audit, Debug, Monitoring and Logging services.

# Helicoper Racing League

Streams races all around the world with live telemetry and predictions

Want to expand use of managed AL and ML services for preidictions - move content closer to users

Don't have real time predictions and capacity to process season-long results

Since they are already a cloud native, there shouldn't need to be a whole lot of advocacy

## Business Requirements

### Ability to expose predictive models to partners

### Increase predictive capabilities during and before races for race results, mechanical failures and sentiment analysis

This would be some combination of Vertex and a data streaming pipeline like Dataflow

### TelemetryIncrease global avaliability and quality of broadcasts

This might be to use better encoding schemes like HLC or Cloud Load Balancer/CDN

### Increase number of concurrent viewers

Cloud Load Balancer

### Minimize operational complexity

Operational complexity means you want completely managed services like Big Query or App Engine.

### Maintain compliance with regulations

Since they conduct an e-commerce business and it features quite heavily in the breif, need to look into how sensitive data scuh as credit card is handled

## merchandising revenue streamTechnical Requirements

More accurate models with greater throughput

Reduce latency

### Increase transcoding performance

- One of the listed benefits of using GPU enhanced GKE is that it can help with compute intensive tasks such as video transcoding. Do note some of the limiting factors when deploying GPUs are that you can't add them to existing node pools, they can't be live migrated and are only supported with N1 machine types.
- GPUs are only available in specific regions and zones as well. So to check, run `gcloud compute accelerator-types list`

Create real-time analytics of viewer consumption patterns and engagements

Create data mart to process large volumes of race data

# Mountkirk Games

This actually has the most interesting use case because they are already on Google Cloud so they are already familiar with the offerings. They even make reference to specific Google products like Cloud Spanner to serve data and Google Kubernetes Engine to run their services. However, the requirements are similarly vague

## Business Requirements

### Minimize costs

- Use preemptible and autoscaling to keep costs low. Store data in BigQuery because that's the one that provides you with a managed database for the lowest cost.
- Use Google's Pricing Calculator to determine prices

### Support rapid iteration of game features

- For Cloud spanner, you want to use separate databases per tenant because as the game is updated, the schema changes, and isolation at the database level can simpify schema updates.

### Optimize for dynamic scaling

- Dynamic scaling can be used with a managed instance group or an autoscaling cluster. Your pick.

### Use managed services and pooled resources

### Minimize costs

- Who doesn't? Preemptible nodes and autoscaling. Do note that for preemptible nodes, smaller nodes have better availability.

## Technical Requirements

### Dynamically scale based on game activity

- Cloud spanner

### Publish scoring data on a near real-time global leaderboard

- Since they are using Cloud Spanner, some means to speed up Cloud spanner is to use a few techniquues such as parametermized queries to speed up frequenty executed queries.
- Secondary indexes can also be used to speed up queries
- Of course, the best answer is from Google themselves
- Make sure that you provision for more than your peak load
{{< bookmark
  url="https://cloud.google.com/architecture/best-practices-cloud-spanner-gaming-database"
  title="Best practices for using Cloud Spanner as a gaming database"
  favicon="https://www.gstatic.com/devrel-devsite/prod/vacc2a2a4a4394c7c42dc62dba69eb022d7680ce4a368d4b28c3e984cc9155a81/cloud/images/favicons/onecloud/super_cloud.png"
  site="Google Cloud"
>}}
### Store game activity logs in structured files for future analysis

- Since it is structured and required for analysis, I would

### Use GPU processing to render graphics server-side

- Since Mountkirk Games already stated that they are on the GKE platform, we can only assume that they want to attach GPU instances to their node pool

# EHR Healthcare

EHR Healthcare has several interesting features that they already have in play.

- Customer-facing applications are web-based and many have recently been containerized to run on a group of Kubernetes clusters - this is a classic lift and shift use-case for moving from on-prem to GKE.
- Data is stored in a mixture of relational and NoSQL databases (MySQL, MS SQL Server - Cloud SQL/Cloud Spanner, how can you migrate? Redis - Memorystore. MongoDB - Datastore)
- Hosting several legacy file- and API-based integrations with insurance providers on-prem with no plans to upgrade. This means that it will be some sort of hybrid architecture where you'll need to link back to the on-prem environment to get access. Cloud VPN, Dedicated/Partner Interconnect)
- Users are managed through Microsoft Active Directory - manage users through Google Cloud (how will you do it?)
- Monitoring is done through various open source tools - this might imply that you can replace it with Google's Monitoring suite, and there might be a need to adjust the alerting policies because they are being sent via email and are often ignored (what other alerting options are there?)

## Business Requirements

### On-board new insurance providers as quickly as possible

- This is unfortunately a little too vague to provide any useful commentary on. A standardized process of on-boarding is the best way to ensure this, but without any details of the process, I'm not sure what Google Cloud service would apply here.

### Minimum 99.9% availability for all customer-facing systems

- No preemptible or services that have no guarantees or SLA

### Centralized visibility and proactive action on system performance and usage

- This points to using the Operations Suite and metrics to keep track of systems and alerting policies to ensure systems perform as expected.

### Reduce latency to all customers

- Multi-region deployment for GKE clusters - since it is web-based there might also be a case for Cloud CDN
- Cloud Spanner

### Regulatory Compliance

- HIPAA - health

### Infrastructure administration costs

- Terraform
- Managed Services

### Make predictions and generate reports on industry trends based on provider data

- Must be able to perform analysis on data - some potential candidates include BigQuery, BigQuery ML, Vertex

## Technical requirements

### Maintain legacy interfaces for on-prem and cloud providers

- This would mean maintaining on-prem services. For existing cloud providers, you might want to use something like Google's Data Transfer service which can connect to AWS S3 buckets.

### Provide a consistent way to manage customer facing applications that are container-based

- GKE

### Provide a secure and high-performance connections between on prem and google cloud

- Dedicated Interconnect - direct physcal connection between on-prem network and Google's network - since Google was chosen to replace their current colocation facilities, i.e. they will be colocating with Google, this imples that Dedicated Interconnect is a likely option.
- Partner Interconnect - On prem to VPC through a supported service provider
- Know what security and bandwidth each connection provides.

### Consistent logging, log retention, monitoring and alerting capabilities

- Operations Suite - provides consistent logging capabilties - just have to install it. You can also collect metrics from on-prem using BindPlane, which can be activated from Google Cloud Marketplace.
- Alerting can be done by Email, Mobile App, PagerDuty, SMS, Webhooks, Pub/Sub and Slack
- Export to coldline storage for log rentention

### Maintain and manage multiple container-based environments

- Istio and Anthos

### Dynamically scale and provision new resouces

- Managed Groups for Compute Engine
- Autoscaling cluster in GKE

### Create interface to ingest and process data from new providers

- This is incredibly generic, but you can combine Cloud Functions with Pub/Sub so that you have a really flexible means to get data from providers (by calling their API endpoints) and feed it into a flexible queue to be consumed by other applications
