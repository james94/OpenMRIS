# Deep Learning Containerization Notes

## Docker for Deep Learning

**Key Concepts:**

- Containerize ML models and dependencies
- Ensure consistency across development and production environments
- Simplify deployment and reproducibility

**Best Practices:**

- Use official deep learning base images (e.g., TensorFlow, PyTorch)
- Minimize image size by removing unnecessary dependencies
- Leverage multi-stage builds for smaller production images

**Dockerfile Example for Medical Imaging:**

```dockerfile
FROM pytorch/pytorch:1.9.0-cuda11.1-cudnn8-runtime

# Install additional dependencies
RUN pip install nibabel pydicom scikit-image

# Copy model code and artifacts
COPY ./model /app/model
COPY ./requirements.txt /app/requirements.txt

# Install project dependencies
RUN pip install -r /app/requirements.txt

# Set working directory
WORKDIR /app

# Command to run the model
CMD ["python", "model/predict.py"]
```

**Docker Commands:**

- Build image: `docker build -t medical-imaging-model:v1 .`
- Run container: `docker run --gpus all medical-imaging-model:v1`
- Push to registry: `docker push your-registry/medical-imaging-model:v1`

## Kubernetes for Deep Learning

**Key Concepts:**

- Orchestrate containerized ML workloads
- Scale training and inference jobs
- Manage GPU resources efficiently

**Essential Kubernetes Resources:**

- **Pods:** Basic unit for running containers
- **Deployments:** Manage pod replicas and updates
- **Services:** Expose pods to network traffic
- **PersistentVolumes:** Store large datasets and model artifacts
- **Jobs:** Run training tasks to completion
- **HorizontalPodAutoscaler:** Scale based on metrics

**Best Practices:**

- Use node selectors or taints/tolerations for GPU allocation
- Implement resource quotas and limits
- Utilize Kubernetes Secrets for sensitive data (e.g., API keys)

**Kubernetes Manifest Example for Medical Imaging Inference:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: medical-imaging-inference
spec:
  replicas: 3
  selector:
    matchLabels:
      app: medical-imaging-inference
  template:
    metadata:
      labels:
        app: medical-imaging-inference
    spec:
      containers:
      - name: medical-imaging-model
        image: your-registry/medical-imaging-model:v1
        resources:
          limits:
            nvidia.com/gpu: 1
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: medical-imaging-inference-service
spec:
  selector:
    app: medical-imaging-inference
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

**Kubernetes Commands:**

- Apply manifest: `kubectl apply -f medical-imaging-deployment.yaml`
- Scale deployment: `kubectl scale deployment medical-imaging-inference --replicas=5`
- View logs: `kubectl logs deployment/medical-imaging-inference`

## Deep Learning Lifecycle with Docker and Kubernetes

1. **Development:**

   - Use Docker to create reproducible environments
   - Leverage Docker Compose for multi-container development setups

2. **Training:**

   - Package training code and dependencies in Docker images
   - Use Kubernetes Jobs for distributed training across GPU nodes

3. **Model Versioning:**

   - Tag Docker images with model versions
   - Use Kubernetes Deployments for rolling updates of models

4. **Inference:**

   - Deploy models as Kubernetes Deployments
   - Use HorizontalPodAutoscaler for scaling based on CPU/GPU utilization

5. **Monitoring:**

   - Implement Prometheus and Grafana for monitoring Kubernetes clusters
   - Use custom metrics for ML-specific monitoring (e.g., inference latency)

6. **CI/CD:**

   - Automate Docker image builds in CI pipelines
   - Use GitOps with tools like ArgoCD for Kubernetes deployments

## Medical Imaging Specific Considerations

- Implement volume mounts for large imaging datasets
- Use init containers for data preprocessing steps
- Consider using Kubernetes CronJobs for periodic model retraining
- Implement DICOM listeners as separate containerized services
- Ensure HIPAA compliance in container and Kubernetes configurations

## Performance Optimization

- Use NVIDIA CUDA base images for GPU acceleration
- Implement data parallelism for distributed training in Kubernetes
- Optimize Docker images for faster startup times in medical emergencies
- Use Kubernetes node affinity for co-locating related services

## Citations

- [1] https://www.datacamp.com/tutorial/containerization-docker-and-kubernetes-for-machine-learning
- [2] https://www.equuscs.com/containerization-deep-learning-ai-workflows/
- [3] https://overcast.blog/mastering-kubernetes-for-machine-learning-ml-ai-in-2024-26f0cb509d81?gi=7b82660c7c88
- [4] https://cloud.google.com/deep-learning-containers/docs/kubernetes-container
- [5] https://duplocloud.com/blog/machine-learning-on-kubernetes/
- [6] https://opensource.com/article/20/9/deep-learning-model-kubernetes
- [7] https://library.fiveable.me/machine-learning-engineering/unit-8/containerization-orchestration-docker-kubernetes/study-guide/iIzILeUsNSOT5kw2
- [8] https://kubernetes.io/index.html

**NOTE**: We would also like to give credit to **perplexity AI**, which we asked questions to help us obtain the Containerization for Deep Learning in Medical Imaging with emphasis on Docker and Kubernetes notes: https://www.perplexity.ai/search/you-are-a-software-deep-learni-1lgw.qDPSn6E0LIeEmwJKg#3
