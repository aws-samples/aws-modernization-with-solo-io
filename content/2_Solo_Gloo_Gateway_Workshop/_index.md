---
title: "Gloo Gateway (Bonus Section)"
chapter: true
weight: 14
---

# Gloo Gateway (Bonus Section)

In some situations, implementing a service mesh might be premature for your environment, but you would still like to leverage the functionality demonstrated in the Gloo Mesh module, focusing solely on ingress without wrapping up microservices. Solo.io offers Gloo Gateway, a product that focuses on ingress traffic without the need to implement a service mesh from day one. In this section of the workshop, we will deploy Gloo Gateway and demonstrate AI integration with the Hugging Face AI service, using the service AI Key and JWT token to ensure that specific users are authorized to leverage the service. This is a bonus module and can be completed if time allows.

![Gloo Gateway Architecture](images/gg-architecture.png)

## Prerequisites and Requirements

- One EKS cluster in any region with a minimum of 2 worker nodes of size 2vCPU and 8GB memory or above
- kubectl CLI configured to communicate with the EKS cluster
- Helm binary present to deploy demo applications
- A basic understanding of containers and Kubernetes

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}
