title: "Lab 6 - AI API Key Handling"
chapter: true
weight: 7
---

## Lab 6 - AI API Key Handling

![Gloo Platform EKS Workshop Architecture Lab 6](/images/gloo-platform-eks-workshop-lab6.png) 

In this lab, we will apply the concepts from "Lab 5 - Authentication / API Key" to the AI API service created in "Lab 4 - AI Integration". This workflow allows for automatic and secure control of the API Token without user interaction with the key, while still protecting the use of the key with user credentials.

1. Define `ExtAuthPolicy` to Require User Credentials for Endpoint Access:

    ```bash
    kubectl apply -f - <<'EOF'
    apiVersion: security.policy.gloo.solo.io/v2
    kind: ExtAuthPolicy
    metadata:
      name: huggingface-apikey
      namespace: gloo-mesh-gateways
    spec:
      applyToRoutes:
        - route:
            labels:
              route: huggingface
      config:
        server:
          name: ext-auth-server
          namespace: gloo-mesh-gateways
          cluster: cluster-1
        glooAuth:
          configs:
          - apiKeyAuth:
              headerName: api-key
              k8sSecretApikeyStorage:
                labelSelector:
                  api-key: api-huggingface
    EOF
    ```

2. Define User Credentials as a Kubernetes Secret:

    For demonstration purposes, user credentials are defined as a Kubernetes secret. In production, external key repositories and vaults should be used.

    ```bash
    kubectl apply -f - <<'EOF'
    apiVersion: v1
    kind: Secret
    metadata:
      name: api-huggingface-key
      labels:
        api-key: api-huggingface
    type: extauth.solo.io/apikey
    data:
      api-key: bXlzZWNyZXRrZXk= # Base64 encoded value "mysecretkey"
    EOF
    ```

3. Confirm Denied Access Without Credentials:

    Verify that access to the service is denied when credentials are not provided.

    ```bash
    curl -I http://$GLOO_AI_GATEWAY/huggingface
    ```

    ![Expected Output](/images/ai_401_output.png)

4. Confirm Access with Correct `api-key`:

    Verify that the correct `api-key` works.

    ```bash
    curl -v -H "api-key: mysecretkey" http://$GLOO_AI_GATEWAY/huggingface
    ```

    ![Expected Output](/images/ai_200_output.png)

5. Transfer Hugging Face API Token to Existing Secret:

    Transfer the Hugging Face API Token, currently stored as an environmental variable, to the existing secret that already has user credentials.

    ```bash
    kubectl patch secret api-huggingface-key \
    --type=json \
    -p='[{"op": "add", "path": "/data/huggingface-api-key", "value": "'$(echo -n $HF_API_TOKEN | base64)'"}]'
    ```

6. Update Auth Policy with API Token Location:

    Update the `ExtAuthPolicy` to define the API Token location.

    ```bash
    kubectl patch extauthpolicy huggingface-apikey \
    -n gloo-mesh-gateways \
    --type=json \
    -p='[{"op": "add", "path": "/spec/config/glooAuth/configs/0/apiKeyAuth/headersFromMetadataEntry", "value": {"x-api-key": {"name": "huggingface-api-key"}}}]'
    ```

7. Define `TransformationPolicy` to Add API Token to Request:

    Instruct Gloo Mesh to add the API Token to the request by defining a `TransformationPolicy`.

    ```bash
    kubectl apply -f - <<'EOF'
    apiVersion: trafficcontrol.policy.gloo.solo.io/v2
    kind: TransformationPolicy
    metadata:
      name: huggingface-transformations
    spec:
      applyToRoutes:
      - route:
          labels:
            route: huggingface
      config:
        request:
          injaTemplate:
            headers:
              Authorization:
                text: 'Bearer {{ huggingface-api-key }}'
            extractors:
              huggingface-api-key:
                header: 'x-api-key'
                regex: '.*'
    EOF
    ```

8. Test the Setup:

    Test the setup by providing user credentials and receiving a response from the AI upstream service, with the proper API Token provided by Gloo Mesh based on the configuration defined in this lab.

    ```bash
    curl http://$GLOO_AI_GATEWAY/huggingface \
        -X POST \
        -d '{"inputs": "Explain in 1-2 sentences why EKS value significantly increases when Gloo Mesh is added"}' \
        -H 'Content-Type: application/json' \
        -H "api-key: mysecretkey"
    ```

    ![Expected Output](/images/ai_api_key.png)

In this lab, we have effectively integrated secure API key handling for AI services within the Gloo Platform on EKS. By implementing authentication policies and leveraging Kubernetes secrets, we've ensured that sensitive AI API tokens are managed securely and automatically. This lab demonstrated how to streamline access to powerful AI capabilities, such as those provided by Hugging Face, without exposing API keys directly to users.

The transformation policy we defined allows Gloo Mesh to handle the token insertion dynamically, ensuring seamless communication with AI services. These practices are crucial for maintaining robust security while leveraging AI and LLM services in modern applications. The skills acquired here will be instrumental in managing secure and efficient access to AI services, enabling you to build intelligent, AI-powered microservices architectures with confidence.