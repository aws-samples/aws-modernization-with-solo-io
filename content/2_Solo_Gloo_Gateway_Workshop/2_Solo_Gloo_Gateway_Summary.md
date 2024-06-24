---
title: "Workshop Summary"
chapter: true
weight: 18
---

## Workshop Summary

Throughout this workshop, we navigated a series of labs that enhanced our proficiency in managing and securing a microservices architecture with Gloo Gateway, focusing on the integration and protection of AI services:

- **Deploying Gloo Gateway**: We began by setting up the Enterprise version of Gloo Gateway, demonstrating its capabilities in handling traffic management for microservices, VMs, and serverless functions.

- **AI Virtual Service Definition**: We created virtual services for integrating AI endpoints like Hugging Face, OpenAI, and Mistral. This showcased Gloo Gateway's flexibility in supporting AI services without necessitating a full service mesh deployment.

- **JWT Token Security**: We implemented security measures using JWT tokens to validate access to AI services, ensuring that only authorized requests are allowed. This step highlighted our ability to enforce robust security policies even in the absence of a service mesh.

- **Flexibility in Deployment**: Gloo Gateway's ability to function effectively without a service mesh was emphasized, providing a scalable solution for organizations not ready to adopt a full service mesh but still wanting the benefits of one.

- **Practical Application and Testing**: Hands-on labs reinforced our understanding and skills, from configuring upstream services to securing them with JWT tokens, culminating in testing and validation using the **curl** command.

The knowledge and skills acquired in this workshop are vital for anyone working with Kubernetes and microservices in a cloud-native environment. By mastering these concepts, you are well-equipped to build, manage, and secure advanced microservices architectures, including those that leverage AI technologies.

Thank you for participating in the Gloo Gateway Workshop. We hope this experience has been insightful and empowering, providing you with valuable tools and knowledge for your future endeavors in cloud-native technologies.

## References

Review the following ways to get additional help, training, and other forms of support as you use Gloo Gateway:

- [Support information Issues](https://docs.solo.io/gateway/latest/support/about/)
- [OSS Github Repository](https://github.com/solo-io/gloo)
- [Solo Slack](http://slack.solo.io/) (to join use this [link](https://docs.solo.io/gloo-edge/latest/support/support-ticket/#slack))
- [Using Gloo Gateway to Support LLM APIs](https://www.solo.io/blog/gloo-gateway-support-llm-apis/)
