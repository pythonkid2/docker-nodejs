# DevOps Assignment 2: Kubernetes - Dynamic Scaling with HPA and Load Testing

## Prerequisites

- Install and setup kubernetes cluster 
- Install Git
   

#### Step 1: Kubernetes Deployment:

1. Clone the provided Node.js codebase from the provided link.

```
git clone https://github.com/pythonkid2/docker-nodejs
```

2. Navigate into the cloned directory.

```
cd docker-nodejs
```

3. Prepare Dockerfile and Kubernetes deployment manifests for the Node.js application.

```
# Dockerfile
FROM node:latest

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "main.js"]
```

Image is builded and pushed to git hub 
```
docker pull mjcmathew/nodjs-app:v1
```

4. Create Kubernetes deployment and service manifests.

```
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodjs-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nodjs-app
  template:
    metadata:
      labels:
        app: nodjs-app
    spec:
      containers:
        - name: nodjs-app
          image: mjcmathew/nodjs-app:v1
          ports:
            - containerPort: 3000
```

```
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nodjs-app-service
spec:
  selector:
    app: nodjs-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```

5. Deploy the application to the local Kubernetes cluster.

```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

#### Step 2: Autoscaling with HPA:

1. Implement Horizontal Pod Autoscaler (HPA) configuration in Kubernetes.

```
# hpa.yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: nodjs-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nodjs-app
  minReplicas: 1
  maxReplicas: 5
  targetCPUUtilizationPercentage: 50
```

2. Apply the HPA configuration.

```
kubectl apply -f hpa.yaml
```

#### Step 3: Resource Limits and Stress Testing:

1. Define resource limitations (CPU and RAM) for the deployed application pods in Kubernetes manifests.

```
# deployment.yaml (update)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodjs-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nodjs-app
  template:
    metadata:
      labels:
        app: nodjs-app
    spec:
      containers:
        - name: nodjs-app
          image: mjcmathew/nodjs-app:v1
          ports:
            - containerPort: 3000
          resources:
            limits:
              cpu: "500m"
              memory: "512Mi"
            requests:
              cpu: "250m"
              memory: "256Mi"
```

2. Use a load testing tool like `hey` or any other to simulate increased traffic and stress the application under load.


To install `hey` on a Linux 64-bit system

1. Download the `hey` binary for Linux 64-bit from the provided link:
   ```
   wget https://hey-release.s3.us-east-2.amazonaws.com/hey_linux_amd64
   ```

2. Make the downloaded binary executable:
   ```
   chmod +x hey_linux_amd64
   ```

3. Move the binary to a directory in your system's PATH to make it accessible from anywhere:
   ```
   sudo mv hey_linux_amd64 /usr/local/bin/hey
   ```

4. Verify the installation by running:
   ```
   hey --version
   ```

 use `hey` to send load to a web application. 

```
hey -z 30s -c 50 http://<your-web-application-url>
```

This command will send load to the specified URL for 30 seconds with a concurrency of 50 requests. 

**Kubernetes Metrics Server Installation**

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

**To monitor scaling behavior**
```
watch kubectl get hpa,pods
```

[link to the video](https://screenrec.com/share/3HJ0YGaUmy)

![image](https://github.com/pythonkid2/docker-nodejs/assets/100591950/3c38bac7-32d0-44c1-b670-c6aa18f12e94)

