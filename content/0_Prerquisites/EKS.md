---
title: "EKS Cluster deployment"
chapter: true
weight: 2
---

## Setting Up Your Environment Variables

Starting with setting up your environment variables is the most efficient approach. Please adjust these variables based on your specific AWS region:

```bash
export AWS_REGION=us-west-2
export EKS_VERSION=1.29
export CLUSTER_NAME=solo-io-gloo-mesh-workshop
NUMBER_NODES=3

# Choose from compatible options - for example, the compute-optimized family with 8 vCPUs and 16.0 GiB of memory
NODE_TYPE="c7i.xlarge" 
```

## Deploy Your Cluster with eksctl

Use the following `eksctl` command to create your cluster. This command initiates the creation of CloudFormation stacks — one for the cluster and another for deploying the nodes:

```bash
eksctl create cluster --region $AWS_REGION \
  --name $CLUSTER_NAME \
  --nodes $NUMBER_NODES \
  --node-type $NODE_TYPE \
  --version=$EKS_VERSION 
```

![eksctl output example](/images/eksctl-output.png)

Deployment will take 10-30 minutes and at the end your kubecontext will be pointing to this instance. 

Now you're ready to dive into the workshop!
