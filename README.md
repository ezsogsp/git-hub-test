<!--Document Template information:
Prepared:Stefano Volpe
Approved:***
Document Name:user-guide-template
Revision: {!.bob/var.user-guide-version!}
Date: {!.bob/var.date!}
-->

# <*Service Name*> User Guide

<*Template Version: {!.bob/var.user-guide-version!}*> <!-- remove this line -->

[TOC]

## Overview

This document provides an overview of the <*Service Name*> Service. It gives a
brief description of its main features and its interfaces.

<*Description of the Service, purpose, main concepts, features, use cases, APIs
etc*>

### Supported Use Cases

This chapter gives an overview of the supported use cases.

 Use Case ID | Use Case Title | Compliance | Maturity
---|---|---|---
 <*UC.ID*> | <*UC Slogan*> | <*Fully supported or list deviations*> | <*Alpha, Beta, Stable*>

### Architecture

<*Brief description of the architecture in sync with included picture which is
stored in same folder as this markdown file. Focus on interfaces*>

The following picture shows the <*Service Name*> Service and its
architectural context.

![Architecture](SERVICENAME_architecture_overview.png)

Figure 1 Architecture view of <*Service Name*>

Interface Logical Name | Interface Realization | Description | Interface Maturity
---|---|---|---
<*INT.NAME*> | <*Link to specification, JSON, documentation, etc.*> | <*Short description of the interface*> | <*Interface Maturity (Alpha, Beta, Stable)*>

### Deployment View

<*Service Name*> is packaged as a Docker container. It supports deployment in
Kubernetes using Helm.

<*Describe the deployment and use a picture to visualize the details. Describe
how it uses Kubernetes resources to give a better understanding for the helm
chart implementation. Describe dependencies to other services.*>

![Deployment Overview](SERVICENAME_deployment_overview.png)

Figure 2 Deployment view of <*Service Name*>

To deploy the Service, refer to the [Deployment section](#deployment), which:

- explains how to get started using the <*Service Name*> Service in the
supported environments.
- specifies configuration options for starting the <*Service Name*> docker
container.

If problems occur when using the service, refer to the [Troubleshooting section](#troubleshooting).

### Dimensioning and Characteristics

#### Dimensioning

<*Describe static information such as formulas and things to consider in
general*>

To handle dimensioning configuration at deployment time,
refer to the [Deployment section](#deployment).

#### Scaling

<*Describe how the service supports scaling from a functional point of view.
Describe if there are any dependant services or other types of resources that
must be considered when scaling is performed. Describe how many instances it
supports using the table below. Note that the Deployment Guide only describes
how to scale from an operational point of view.*>

Scaling Supported (Yes/No) | Minimum number of instances | Maximum number of recommended instances
---|---|---
<*State Yes or No*> | <*What is minimum number of supported instances (normally 1)*> | <*What is max number of recommended instances*>

#### Resilience

<*Describe from a functional view how it is designed to support resilience. How
does the service achieve high availability*>

#### Upgrade - In Service Software Upgrade

<*Describe how the service supports upgrades, that is how the service fulfills
the ISSU requirement. Describe from a function point of view how ISSU is
supported and what a service unavailability would mean for this particular
service. Do not include practical commands, that is the scope of the Deployment
section*>

## Deployment

This section describes the operational procedures for how to deploy and upgrade
the <*Service Name*> Service in a Kubernetes environment with Helm. It also
covers hardening guidelines to consider when deploying this service.

### Prerequisites

<*List of prerequisites for service deployment.<br/>
e.g.:*

-  *A running Kubernetes environment with helm support, some knowledge
    of the Kubernetes environment, including the networking detail, and
    access rights to deploy and manage workloads.*

-   *Access rights to deploy and manage workloads.*

-   *Availability of the kubectl CLI tool with correct authentication
    details. Contact the Kubernetes System Admin if necessary.*

-   *Availability of the helm package.*

-   *Availability of Helm charts and Docker images for the service and
    all dependent services.*>

### Custom Resource Definition (CRD) Deployment

Custom Resource Definitions (CRD) are cluster-wide resources which must be
installed prior to the Generic Service deployment.
The service that requires the latest CRD charts version set which CRD charts
version to use in the deployment.

>Note! The release name chosen for the CRD charts in the K8s deployment must be
kept and cannot be changed later.

Helm ALWAYS requires a namespace to deploy a chart and the namespace to be used
for the same per-service CRDs chart must be created before the CRDs charts are
loaded.

>Note! The upgrade of the CRDs must be done before deploying a service that
requires additional CRDs or a CRD version newer than the one already deployed on
the K8s cluster.

The CRD chart are deployed as follows:

1. Download the latest applicable CRD charts files from the helm charts
   repository.

2. Create the `namespace` where CRD charts will be deployed in case it doesn't
   already exist. This step is only done once on the K8s cluster.

    ```
    kubectl create namespace <NAMESPACE-FOR-CRDs>
    ```

3. Deploy the CRD using `helm upgrade` command:

    ```
    helm upgrade --install --atomic <release name> <CRD chart with version> --namespace <NAMESPACE-FOR-CRDs>
    ```

4. Validate the CRD installation with the `helm ls` command:

    ```
    helm ls -a
    ```

In the table locate the entry for the `<release name>`,  validate the
`NAMESPACE` value and check that the STATUS is set to `deployed`.
When the output is validated continue with the installation of the helm charts.

<*To verify that the CRD was installed correctly, the logs of that job can
be used instead of the step above. In this case it should be listed which log
entries indicate a successful CRD charts deployment*>

### Deployment in a Kubernetes Environment Using Helm

This section describes how to deploy the service in Kubernetes using Helm and
the `kubectl` CLI client. Helm is a package manager for Kubernetes that
streamlines the installation and management of Kubernetes applications.

#### Preparation

Prepare helm chart and docker images. Helm chart in the following link
can be used for installation:

<*Link to helm chart package in ARM*>

#### Pre-Deployment Checks for <*Service Name*>

<*List all prerequisites for service deployment: needed K8s cluster
configurations resources that need to be created in advance, check to perform,
etc.*>

Ensure the following:

- The <RELEASE_NAME> is not used already in the corresponding cluster.
Use `helm list` command to list the existing deployments (and delete previous
deployment with the corresponding <RELEASE_NAME> if needed).

- The same namespace is used for all deployments.

- <*...*>

#### Helm Chart Installations of Dependent Services

<*Repeat for each dependent service. Add a step for each service needed and
refer to that service Deployment Guide for details. Do not duplicate information
such as helm install command, instead refer. Do add information that is
important to provide during installation of the service for this context, for
example certain settings that must be enabled. If no dependency to other
services exists, then remove this chapter.*>

#### Helm Chart Installation of <*Service Name*> Service

>**_Note:_** Ensure all dependent services are deployed and healthy before you
>continue with this step (see previous chapter).

Helm is a tool that streamlines installing and managing Kubernetes
applications. <*Service Name*> can be deployed on Kubernetes using
Helm Charts. Charts are packages of pre-configured Kubernetes resources.

Users can override the default values provided in the values.yaml template of
the helm chart. The recommended parameters to override are listed in the
following section: [Configuration Parameters](#configuration-parameters).

##### Deploy the <*Service Name*> Service

<*Describe installation steps with proper sample commands to deploy and
configure the service.
Try to provide several examples, like:*

- *deployment with mandatory parameters only;*
- *deployment with some optional parameters.*

*If the service supports deployment time parameter to enable/disable optional
features or interaction with other services, provide a sample command for each
of these deployment scenarios*>

Install the <*Service Name*> on the Kubernetes cluster by using the
helm installation command:

```text
helm install <CHART_REFERENCE> --name <RELEASE_NAME> --namespace <NAMESPACE> [--set <other_parameters>]
```

The variables specified in the command are as follows:

- `<CHART_REFERENCE>`: A path to a packaged chart, a path to an unpacked chart
directory or a URL.

- `<RELEASE_NAME>`: String value, a name to identify and manage your helm chart.

- `<NAMESPACE>`: String value, a name to be used dedicated by the user for
deploying own helm charts.

##### Verify the <*Service Name*> Service Availability

<*Describe the steps to verify the successful deployment of the service.<br/>
e.g<br/>
To verify whether the deployment is successful, do as follows:*

*1.  Check if the chart is installed with the provided release name and
    in related namespace by using the following command:*

```text
$helm ls <namespace>
```

  *Chart status should be reported as "DEPLOYED".*

*2.  Verify the status of the deployed helm chart.*

```text
$helm status <release_name>
```

  *Chart status should be reported as "DEPLOYED". All Pods status should be
  reported as "Running" and number of Deployment Available should be the
  same as the replica count.*

*3.  Verify that the pods are running
    by getting the status for your pods.*

```text
$kubectl get pods --namespace=<namespace> -L role
```

  *For example:*

```text
$helm ls example
$helm status examplerelease
$kubectl get pods --namespace=example -L role
```

  *All pods status should be "Running". All containers in any pod should
  be reported as "Ready". There is one POD marked as the master role and
  the other PODs are marked as the replica role.*>

### Configuration Parameters

#### Mandatory Configuration Parameters

The parameters in following table are mandatory to set at deployment time.
If not provided, the deployment will fail. There are no default values
provided for this type of parameters.

| Variable Name | Description |
|---|---|
| <*variable name*>  | <*variable description*>  |
| ...  | ...  |

#### Optional Configuration Parameters

Following parameters are not mandatory. If not explicitly set
(using the --set argument), the default values provided
in the helm chart are used.

| Variable Name | Description | Default Value |
|---|---|---|
| <*variable name*>  | <*variable description*>  | <*variable default value*>  |
| ...  | ...  | ... |

### Service Dimensioning

The service provides by default resource request values and resource limit
values as part of the Helm chart. These values correspond to a default size for
deployment of an instance. This chapter gives guidance in how to do service
dimensioning and how to change the default values when needed.

#### Override Default Dimensioning Configuration

If other values than the default resource request and default resource limit
values are preferred, they must be overridden at deployment time.

Here is an example of the `helm install` command where resource requests and
resource limits are set:

```text
helm install https://arm.rnd.ki.sw.ericsson.se/artifactory/proj-adp-helm-dev-generic-local/some/repo/path/eric-data-my-service/eric-data-my-service-1.0.0-999.tgz --name eric-data-myservice --namespace test-deployment-namespace --set <*ADD request and limit parameters valid for this service*>
```

#### Use Minimum Configuration per Service Instance

This chapter specifies the minimum recommended configuration per service
instance. <*Columns not applicable in table below should be removed*>

| Resource Type (Kubernetes Service) | Resource Request Memory | Resource Limit Memory | Resource Request CPU | Resource Limit CPU | Resource Request Disk | Resource Limit Disk |
|---|---|---|---|---|---|---|
| <*Resource/Service*> | <*minimum memory to request*> | <*resource limit for memory that corresponds to the min configuration*> | <*minimum CPU to request*> | <*resource limit for CPU that corresponds to the min configuration*> | <*resource request for Disk*> | <*resource limit for Disk*>|

To use minimum configuration, override the default values for resource requests
and resource limits in the helm chart at deployment time.

#### Use Maximum (Default) Configuration per Service Instance

The maximum recommended configuration per instance is provided as default in the
Helm chart. Both Resource Request values and Resource Limit values are included
in the helm charts.

### Hardening

The service is by default pre-hardened. No additional hardening is required.
The following pre-hardening actions have been made:

<*List specific pre-hardening actions made for this service*><br/>
<*See [here](https://confluence.lmera.ericsson.se/display/AGW/Hardening+Guidelines)
for more details*>

### Upgrade Procedures

>**_Note:_** If any chart value is customized at upgrade time through the
>"--set" option of the "helm upgrade" command, all other previously customized
>values will be replaced with the ones included in the new version of the chart.
>To make sure that any customized values are carried forward as part of the
>upgrade, consider keeping a versioned list of such values. That list could be
>provided as input to the upgrade command in order to be able to use the "--set"
>option without side effects.

<*Add upgrade procedure specific information in this document,
for example something to be considered related to a certain deployment
parameter value*>

## Security Guidelines

<*For Security Guidelines details, see
[here](https://confluence.lmera.ericsson.se/display/AGW/Security+User+Guide)*>

### Operative Tasks

<*Describe the operative tasks that are valid for this service (if
applicable).*>

<*If not applicable, state "This service does not include any
operative tasks."*>

### External Ports

<*This section is mandatory. List all the services, ports, and protocols that
are used by the product. If no services, ports, or protocols are used by the
product, write None and remove the remaining rows.*>

The following ports are exposed to the outside of the cluster:

| Service or Interface name | Protocol | IP Address Type | Port | Transport Protocol | IP Version |
|---|---|---|---|---|---|
|<*Service or interface name*>| <*Application Protocol*> | <*IP Address Type*> | <*Port number*> | <*Transport Protocol*> | <*IP Version*> |
|...|...|...|...|...|...|

### Internal Ports

<*This section is mandatory. List all the services, ports, and protocols that
are used by the product. If no services, ports, or protocols are used by the
product, write "None" and remove the remaining rows.*>

The following ports are cluster internal ports and not exposed to the outside:

| Service or Interface name | Protocol | IP Address Type | Port | Transport Protocol | IP Version |
|---|---|---|---|---|---|
|<*Service or interface name*>| <*Application Protocol*> | <*IP Address Type*> | <*Port number*> | <*Transport Protocol*> | <*IP Version*> |
|...|...|...|...|...|...|

## Privacy User Guidelines

<*There are different content to include depending of the PTI value received
during RA & PIA evaluation. Three cases exist: PTI = 0, 1 < PTI < 180 and PTI =>
180.  The PTI value is calculated by completing the [Personal Data
Classification
instruction](http://erilink.ericsson.se/eridoc/erl/objectId/09004cff88c640c8?docno=LMF-14:001078Uen&action=approved&format=excel12book).
The PTI value must be reported in PRIM as an Evaluation Index (EI).  For more
details, refer to the [Sales Compliance Evaluation Index (EI) in
PRIM](https://erilink.internal.ericsson.com/eridoc/erl/objectId/09004cff8b9af833?docno=EAB-13:019844Uen&format=msw8)
document.*>

---
<*If PTI == 0, include following statement and report value in PRIM, see [Sales
Compliance Evaluation Index (EI) in
PRIM](https://erilink.internal.ericsson.com/eridoc/erl/objectId/09004cff8b9af833?docno=EAB-13:019844Uen&format=msw8)
document*></br>
The service does not collect, store or process personal data on its own.

---
<*If 0 <= PTI < 180, List and classify the personal data items identified during
the completion of the [Personal Data Classification
instruction](http://erilink.ericsson.se/eridoc/erl/objectId/09004cff88c640c8?docno=LMF-14:001078Uen&action=approved&format=excel12book).
For example:*

| Personal Data Category | Personal Data Item |
|---|---|
| Basic Data| IP Address, MSISDN, SIP URI |
| Sensitive data (identifiable user activity) | Event Monitoring (Event Based Monitoring, Call Trace Recordings, General Performance Event Handling …) |

*Report value in PRIM, see [Sales Compliance Evaluation Index (EI) in
PRIM](https://erilink.internal.ericsson.com/eridoc/erl/objectId/09004cff8b9af833?docno=EAB-13:019844Uen&format=msw8)
document*>

---
<*If PTI >= 180, Create a Privacy User Guide and the document must contain:*

- *An overview of all the privacy related functionality supported by the
product.*
- *An overview of the features which have a privacy impact because of the
personal data processed when using such features.*
- *Clear indication of the intended use of the product features and
instructions/procedures for operating the privacy functionality of the product.*
- *Instructions/information how to maintain the privacy status of the product,
i.e. instructions for the privacy configuration of the product in the daily
O&M activities.*
- *The default values (factory settings) of the privacy parameters,
when applicable.*
- *Listing and classification of the personal data processed by the product.*

*Report value in PRIM, see [Sales Compliance Evaluation Index (EI) in
*PRIM](https://erilink.internal.ericsson.com/eridoc/erl/objectId/09004cff8b9af833?docno=EAB-13:019844Uen&format=msw8)
*document*>

## Operation and Maintenance

<*In addition to the sections regarding the Core FA's operations reported below,
include here the subsections to describe any relevant service specific
operation*>

{!fragments/performance_mgmt_fragment.md!}

### Backup and Restore

<*If Backup and Restore is not supported by the service, state the following:*>
The service does not support any kind of backup and restore operation.

<*If the service supports Backup and Restore functionality, describe in this
section the service specific operations needed to perform to execute
backup and restore tasks (if any). In any case, refer to the relevant
Backup and Restore Orchestrator Service instruction,
for the general instructions on backup and restore*>

### Scaling

<*Add scaling procedure specific information in this document, for example
something to be considered related to a certain deployment parameter value*>

## Troubleshooting

This section describes the troubleshooting functions and procedures for
the <*Service Name*>. It provides the
following information:

-   Simple verification and possible recovery.

-   The required information when reporting a bug or writing a support case,
    including all files and system logs that are needed.

-   How to retrieve the above information from the system.

### Prerequisites

-   `kubectl` CLI tool properly configured

### Installation

<*Suggest procedure to recover a failed installation and collect logs*>

### Deletion of Release

<*Suggest procedure to recover a failure while deleting a release
 and collect logs*>

### Health checks

<*Suggest procedures to verify the health of the service to verify it is working
as expected.*>

### Enable debug logging

<*Describe how to enable debug logging if possible. Please note that this should
not be a standard procedure since CICD automatically deploys and the logs
available from the automation should be sufficient.   Only enable debug logging
in order to troubleshoot a problem that can be reproduced. Debug logging may
impact performance.*>

### Log Categories

Log Categories are used to support automatic filtering which enable a
possibility to support AI and machine learning. In the table below the log
categories provided by the service are listed.

| Category Name     | Security Log | Description              |
| ----------------- | -------------| ------------------------ |
| <*category_name*> | <*yes_no*>   | <*category_description*> |
| ... | ... | ... |

<*Instructions:   The Category Name must follow the [Log General Design
Rules](https://confluence.lmera.ericsson.se/display/AA/LOG+General+Design+Rules)
and therefore include the short name of the functional area and nature of the
logs in the category. The Functional Area short name to be used are the
Functional Area Acronyms listed in the [ADP FA
Invetory](https://confluence.lmera.ericsson.se/display/AA/FA+Inventory). Each
microservices log categories includes The combination of the FA short name and
the nature of the log category shall be separated by dash.   Example of category
names: IAM-token-generation, KM-genkey-issuecert.*>

### Data Collection

- The logs are collected from each pod using command:

```text
kubectl exec <pod name> --namespace=<pod's namespace> \
collect_logs > <log file name>.tgz
```

- The detailed information about the pod are collected using command:

```text
kubectl describe pod <pod name> --namespace=<pod's namespace>
kubectl exec <pod-name> --namespace=<pod's namespace> env
```

<*Describe any additional procedure (if any) to collect service specific data.*>

### Bug Reporting and Additional Support

Issues can be handled in different ways, as listed below:

-   For questions, support or hot requesting, see
    Additional Support.

-   For reporting of faults, see Bug Reporting.

#### Additional Support

<*Provide a link to the support form to fill*>
If there are <*Service Name*> Service support issues, use the [JIRA][jira].

#### Bug Reporting

If there is a suspected fault, report a bug. The bug report must
contain specific <*Service Name*> Service information and all
applicable troubleshooting information highlighted in the
[Troubleshooting](#troubleshooting), and [Data Collection](#data-collection).

Indicate if the suspected fault can be resolved by restarting the pod.

<*Provide a link to the tool (usually JIRA) used to report and track bugs
on the service.*>

<*Provide some guidelines on how to fill the form to report the bug.
For example, specify the following for Generic Services:*

*When reporting a bug for the &lt;Service Name&gt; Service, specify
the following in the JIRA issue:*

- *Issue type: Bug*
- *Component: &lt;Service Name&gt;*
- *Reported from: the level at which the issue was originally detected
  (ADP Program, Application, Customer, etc.)*
- *Application: identity of the application where the issue was observed
  (if applicable)*
- *Business Impact: the business impact caused by the issue for the affected
  users*>

### Recovery Procedure

This section describes how to recover the service in case of malfunction.

<*Create a subsection for any possible recovery procedure that can be applied
to recover the service. Restarting the pod and Data recovery reported below
are some common scenarios, that might not be applicable for all services*>

#### Restarting the pod

<*Describe how to restart pods if that could resolve problem.*>

#### Data Recovery

<*Describe the recovery procedure. For example, how to recover a data
service using a restore procedure.*>

### KPIs troubleshooting

#### <*Title of the problem*>

##### Description

<*Description of the problem.*>

##### Procedures for possible fault reasons and solutions

###### <*Title of handled fault reason \#1*>

Prerequisites

- <*Prerequisite for starting the procedure*>

Steps

1. <*First step of action*>
2. <*Second step of action*>
3. ...

###### <*Title of handled fault reason \#2*>

Prerequisites

- <*Prerequisite for starting the procedure*>

Steps

1. <*First step of action*>
2. <*Second step of action*>
3. ...

...

### Alarm Handling

<*Provide a list of the possible alarms the service can raise and refer to the
dedicated OPI for problem resolution.<br/>
Omit this section if the service doesn't raise any alarm.*>

### Known Issues

<*When applicable, this section shall list the most common problems that
can occur and the instructions to avoid them*>

## References

[ADP Generic Services Support JIRA][jira]

[jira]: https://cc-jira.rnd.ki.sw.ericsson.se/projects/GSSUPP
