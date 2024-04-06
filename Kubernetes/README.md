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

```
hey -z 30s -c 50 http://<public-ip>:3000
```


+++

### 1. Kubernetes Deployment:

1. Clone the provided Node.js codebase from the provided link.
2. Prepare Dockerfile and Kubernetes deployment manifests (deployment.yaml, service.yaml) for the Node.js application.
3. Create a local Kubernetes cluster using Minikube or Docker Desktop.
4. Deploy the application to the local Kubernetes cluster.

### 2. Autoscaling with HPA:

1. Implement Horizontal Pod Autoscaler (HPA) configuration in Kubernetes.
2. Configure HPA to scale the number of application pods based on CPU or memory usage metrics.
3. Test the autoscaling behavior by generating load on the application.

### 3. Resource Limits and Stress Testing:

1. Define resource limitations (CPU and RAM) for the deployed application pods in Kubernetes manifests.
2. Use a load testing tool like `hey` or any other to simulate increased traffic and stress the application under load.
3. Analyze the application's performance metrics during stress testing.

### 4. Monitoring and Documentation:

1. Record a screencast demonstrating the load test execution, showcasing the application scaling up and down in response to changing load.
2. Capture screenshots for key stages of the deployment, autoscaling behavior, and load test results.

### 5. Version Control and Sharing:

1. Upload the complete codebase, including Kubernetes deployment manifests, to a public GitHub repository.
2. Update the repository's README.md file with documentation of the project setup, including instructions on running the deployment and load test locally, and links to the video recording and screenshots.

#### Success Criteria:

- A functional Kubernetes deployment for the Node.js application.
- HPA configuration that automatically scales the application based on resource usage.
- Successful execution of the load test with a recorded screencast demonstrating scaling behavior.

#### Evaluation Criteria:

- Proficiency in Kubernetes deployment and configuration.
- Implementation of autoscaling with HPA.

Now, let's break down each step into detailed instructions:
