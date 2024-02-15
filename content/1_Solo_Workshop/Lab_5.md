---
title: "Lab 5 - Zero Trust"
chapter: true
weight: 6
---

## Lab 5 - Zero Trust

![Gloo Platform EKS Workshop Architecture Lab 5](/images/gloo-platform-eks-workshop-lab5.png)

Let's enforce a **Zero Trust** networking approach where all inbound traffic to any applications is denied by default.

1. Add a default deny-all policy to the backend-apis-team workspace:

    ```yaml
    cat << EOF | kubectl apply -f -
    apiVersion: security.policy.gloo.solo.io/v2
    kind: AccessPolicy
    metadata:
      name: allow-nothing
      namespace: online-boutique
    spec:
      applyToWorkloads:
      - selector:
          namespace: online-boutique
      config:
        authn:
          tlsMode: STRICT
        authz: {}
    EOF
    ```

2. Refresh the Online Boutique webpage (**echo http://$GLOO_GATEWAY**). You should see an error with message **"RBAC: access denied"**

3. Add AccessPolicy to explicitly allow traffic between the gateway and the frontend application:

    ```yaml
    kubectl apply -f - <<EOF
    apiVersion: security.policy.gloo.solo.io/v2
    kind: AccessPolicy
    metadata:
      name: frontend-api-access
      namespace: online-boutique
    spec:
      applyToDestinations:
      - selector:
          labels: 
            app: frontend
      config:
        authz:
          allowedClients:
          - serviceAccountSelector:
              labels:
                app: istio-ingressgateway
              namespace: gloo-mesh-gateways
    EOF
    ```

3. Add AccessPolicy to explicitly allow traffic between the microservices online-boutique workspace. As you can see, these policies can be very flexible.

    ```yaml
    kubectl apply -f - <<EOF
    apiVersion: security.policy.gloo.solo.io/v2
    kind: AccessPolicy
    metadata:
      name: in-namespace-access
      namespace: online-boutique
    spec:
      applyToDestinations:
      - selector:
          namespace: online-boutique
      config:
        authz:
          allowedClients:
          - serviceAccountSelector:
              namespace: online-boutique
    EOF
    ```
4. Refresh the page (**echo http://$GLOO_GATEWAY**) again. You should get the store home page back.

In this lab, we effectively implemented a Zero Trust network security model, where we began by denying all inbound traffic by default. Through careful configuration of AccessPolicy rules, we selectively allowed necessary communication between the gateway, the frontend, and other microservices within the online-boutique workspace. This approach not only bolstered our network's security but also demonstrated the practicality and flexibility of Zero Trust principles in a cloud-native ecosystem.

