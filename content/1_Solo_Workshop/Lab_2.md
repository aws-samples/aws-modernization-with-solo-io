---
title: "Lab 2 - Deploy & Expose Online Boutique Sample Application"
chapter: true
weight: 3
---

## Lab 2 - Deploy & Expose Online Boutique Sample Application

![Gloo Platform EKS Workshop Architecture Lab 2](/images/gloo-platform-eks-workshop-lab2.png)

Deploy the Online Boutique microservices to the **online-boutique** namespace.

```sh
kubectl create namespace online-boutique
kubectl label ns online-boutique istio-injection=enabled
helm upgrade --install online-boutique --version "5.0.0" oci://us-central1-docker.pkg.dev/solo-test-236622/solo-demos/onlineboutique \
  --create-namespace \
  --namespace online-boutique
```

To capture the traffic coming to the Gateway and route them to your applications, you need to use the **VirtualGateway** and **RouteTable** resources.

**VirtualGateway** represents a logical gateway configuration served by Gateway workloads. It describes a set of ports that the virtual gateway listens for incoming or outgoing HTTP/TCP connections, the type of protocol to use, SNI configuration etc.

**RouteTable** defines one or more hosts and a set of traffic route rules to handle traffic for these hosts. The traffic route rules can be *delegated* to other RouteTable based on one or more given hosts or specific paths. This allows you to create a hierarchy of routing configuration and dynamically attach policies at various levels. 


1. Let's start by assuming the role of a Ops team. Configure the Gateway to listen **on port 80** and create a generic **RouteTable** that further delegates the traffic routing to RouteTables in other namespaces.

    ```yaml
    kubectl apply -f - <<EOF
    apiVersion: networking.gloo.solo.io/v2
    kind: VirtualGateway
    metadata:
      name: ingress
      namespace: gloo-mesh-gateways
    spec:
      workloads:
        - selector:
            labels:
              app: istio-ingressgateway
            namespace: gloo-mesh-gateways
      listeners: 
        - http: {}
          port:
            number: 80
          allowedRouteTables:
            - host: '*'
              selector:
                namespace: gloo-mesh-gateways
    ---
    apiVersion: networking.gloo.solo.io/v2
    kind: RouteTable
    metadata:
      name: ingress
      namespace: gloo-mesh-gateways
    spec:
      hosts:
        - '*'
      virtualGateways:
        - name: ingress
          namespace: gloo-mesh-gateways
      workloadSelectors: []
      http:
        - name: application-ingress
          labels:
            ingress: all
          delegate:
            routeTables:
            - namespace: online-boutique
    EOF
    ```

2. The Dev team can now write their own RouteTables in their own namespace. Create a RouteTable to send traffic that matches URI **prefix: /** to the frontend application.

    ```yaml
    kubectl apply -f - <<EOF
    apiVersion: networking.gloo.solo.io/v2
    kind: RouteTable
    metadata:
      name: frontend
      namespace: online-boutique
    spec:
      workloadSelectors: []
      http:
        - matchers:
          - uri:
              prefix: /
          name: frontend
          labels:
            route: frontend
          forwardTo:
            destinations:
              - ref:
                  name: frontend
                  namespace: online-boutique
                port:
                  number: 80
    EOF
    ```

3. Visit the online boutique application in your browser:
    ```sh
    export GLOO_GATEWAY=$(kubectl -n gloo-mesh-gateways get svc istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].*}')
    printf "\n\nGloo Gateway available at http://$GLOO_GATEWAY\n"
    ```

![Online Boutique](/images/online-boutique-1.png)

In this lab, we successfully deployed and exposed the Online Boutique Sample Application. By utilizing the VirtualGateway and RouteTable resources, we have established a foundational understanding of how to manage traffic in a microservices architecture. This knowledge is crucial for effectively routing workloads, which is the focus of our next lab.
