---
title: Migrating Azure Pipelines from Classic to YAML
date: "2021-10-19"
template: "post"
draft: true
slug: "/posts/migrate-az-pipelines-from-classic-to-yaml/"
category: "DevOps"
socialImage: /media/lazy.png
tags:
  - Azure Devops
  - Azure Pipelines
  - Devops
  - CI/CD
description: "This post summarizes the experience of migrating Classic Azure Pipelines to YAML."
---
![Be Lazy](./lazy.png)

## CI/CD Pipelines as Code

If you have started using Azure DevOps several years ago (maybe its name was Visual
Studio Team Services by then), then you are probably still using the [Classic
Build and
Release](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops#define-pipelines-using-the-classic-interface)
features to fulfill your CI/CD needs. There is nothing wrong with these Classic
Pipelines but the trend over the years has been to follow the **Everything as
Code** approach, including **CI/CD Pipelines as Code**, and [Azure Pipelines also supports it by using YAML](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops#define-pipelines-using-yaml-syntax).

There are obvious advantages in having CI/CD Pipelines as Code, where its
definitions live in a source control repo, typically as YAML text files:

- we can use the same code flow for CI/CD, for example using pull requests to make a
  change in the pipeline and require a review from a specific group of
  people;
- it's easier to check the history of changes
- we can use the same tooling for source control
- it's easier to reuse and share pipelines common blocks;
- it's easier to collaborate on pipeline definitions

Over the last few days I have been working in a migration process to transform
Classic Builds and Releases into YAML Pipelines.

## Unify Build and Release Pipelines

In the classic approach the Build and the Release are two different pipelines. The
Classic Build doesn't support multi stages and typically finishes by
publishing the build output artifacts. 

TODO: Put image here of Build

The build artifacts are then consumed by a new Release where we have the
different stages (DEV, QA, PROD, etc.) to where we want to promote to.

TODO: Put image here of Release 

The YAML approach supports multi stages which allows us to have in a single
pipeline the Build (as the first stage) and each of the deployment stages (DEV,
QA, PROD. etc.), and we can specify dependencies between stages. The main
difference is that in the YAML approach we can have just a pipeline definition instead of
of the two, Build definition and Release definition.

TODO: Put image here of a single pipeline

## Convert Task Groups into Templates

Using the Classic approach, the team has built several Task Groups over the years. 

TODO: Put image here

Using the YAML approach these task groups were converted into YAML templates. I have decided also to use an isolated git repository to host all these templates to allow to reuse them when building different repos.

TODO: Put image here

## Use Environments

The YAML approach supports the concept of Environment which is very useful: create an Environment, define constraints on it such as Approvals, add resources to it such as virtual machines, and then use the deployment task specifying the Environment just created.

TODO: Put images here of environments

## Limitations

We found also some limitations regarding Trigger Manual Deployments and Notifications.

### Trigger Manual Deployments

### Notifications