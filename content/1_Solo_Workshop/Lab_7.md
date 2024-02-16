---
title: "Lab 7 - Dashboard"
chapter: true
weight: 8
---

## Lab 7 - Dashboard

Let's conclude this workshop by looking at the Gloo Dashboard again and exploring it's various features

1. Launch the dashboard:
  
     ```
     meshctl dashboard
     ```
  
2. The homepage shows an at-a-glance look at the health of workspaces and clusters that make up your Gloo setup. In this workshop, we only used a single cluster and a single workspace, but Gloo Platform is intended to scale to multiple clusters and provide isolation between teams using Workspaces. 

   ![](/images/dashboard-home.png)

3. The Policies page shows the various Access and Traffic Policies that we defined in this workshop.
   ![](/images/dashboard-policy.png)

4. The Debug page shows the Istio or Envoy configuration that was _generated_ by Gloo. Review the configuration of translated Istio resources to help debug issues.
   ![](/images/dashboard-debug.png)

5. The Graph page is used to visualize the network traffic that enters your cluster in a graph that maps out all the nodes by workspace, namespace, or cluster.
   ![](/images/dashboard-graph.png)

This lab, as the final chapter of our Gloo Platform EKS Workshop, brought everything together by showcasing the Gloo Dashboard, an integral component for monitoring and managing the Gloo environment. Through this lab, you've had the opportunity to explore various dashboard features, giving you insights into the health of workspaces and clusters, the policies we've implemented throughout the workshop, and valuable debugging and network traffic visualization tools.
