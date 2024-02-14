---
title: "Gloo Platform EKS Workshop"
chapter: true
weight: 1
---

# Introduction

In this workshop, we will deploy Gloo Platform on a single cluster and a subset of its features. This entire workshop can be completed in less than an hour.

![Gloo Platform EKS Workshop Architecture](images/gloo-platform-eks-workshop.png)

## Prerequisites and Requirements

- One EKS cluster in any region with a minimum of 3 worker nodes of size 4vCPU and 16GB memory and above
- kubectl CLI configured to communicate with the EKS cluster
- helm binary is present to deploy demo application
- A basic understanding of containers and Kubernetes

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}
