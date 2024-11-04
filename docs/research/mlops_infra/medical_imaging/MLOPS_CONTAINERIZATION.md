# MLOps Containerization Notes

## Data Preparation

**Docker:**

- Create a Dockerfile for data preprocessing:

  ```dockerfile
  FROM python:3.8
  RUN pip install pydicom numpy scipy
  COPY preprocess.py /app/
  WORKDIR /app
  CMD ["python", "preprocess.py"]
  ```

- Use volume mounts to access large medical imaging datasets

**Kubernetes:**

- Use PersistentVolumes for storing and accessing large datasets
- Implement data preprocessing as Kubernetes Jobs

## Model Development

**Docker:**

- Create a development environment Dockerfile:

  ```dockerfile
  FROM tensorflow/tensorflow:2.6.0-gpu
  RUN pip install pydicom nibabel scikit-image
  COPY model_dev.py /app/
  WORKDIR /app
  CMD ["python", "model_dev.py"]
  ```

- Use Docker Compose for multi-container development environments

**Kubernetes:**

- Deploy Jupyter Notebooks using Kubernetes for interactive development
- Use ConfigMaps to manage model hyperparameters

## Training

**Docker:**

- Build a training Dockerfile:

  ```dockerfile
  FROM pytorch/pytorch:1.9.0-cuda11.1-cudnn8-runtime
  COPY train.py /app/
  COPY requirements.txt /app/
  RUN pip install -r /app/requirements.txt
  WORKDIR /app
  CMD ["python", "train.py"]
  ```

- Utilize NVIDIA Container Toolkit for GPU support

**Kubernetes:**

- Use Kubernetes Jobs for running training tasks
- Implement distributed training with Kubernetes StatefulSets
- Utilize Kubernetes GPU operator for GPU resource management

## Evaluation

**Docker:**

- Create an evaluation Dockerfile:

  ```dockerfile
  FROM python:3.8
  COPY evaluate.py /app/
  COPY requirements.txt /app/
  RUN pip install -r /app/requirements.txt
  WORKDIR /app
  CMD ["python", "evaluate.py"]
  ```

**Kubernetes:**

- Use Kubernetes CronJobs for periodic model evaluation
- Implement A/B testing using Kubernetes Deployments and Services

## Deployment

**Docker:**

- Build a model serving Dockerfile:

  ```dockerfile
  FROM tiangolo/uvicorn-gunicorn-fastapi:python3.8
  COPY ./app /app
  COPY requirements.txt /app/
  RUN pip install -r /app/requirements.txt
  ```

- Use multi-stage builds to reduce final image size

**Kubernetes:**

- Deploy models using Kubernetes Deployments
- Implement rolling updates for zero-downtime model updates
- Use Horizontal Pod Autoscaler for scaling based on CPU/memory usage

## Monitoring

**Docker:**

- Include monitoring tools in your Dockerfile:

  ```dockerfile
  FROM prom/prometheus
  COPY prometheus.yml /etc/prometheus/
  ```

**Kubernetes:**

- Deploy Prometheus and Grafana for monitoring
- Use Custom Resource Definitions (CRDs) for defining monitoring rules

## MLOps Lifecycle with Docker and Kubernetes

1. **Data Ingestion and Preprocessing:**

   - Use Kubernetes Jobs to run Docker containers for data preprocessing
   - Store preprocessed data in PersistentVolumes

2. **Model Development:**

   - Develop models in containerized Jupyter Notebooks deployed on Kubernetes
   - Version control your Dockerfiles and Kubernetes manifests

3. **Training:**

   - Run training jobs as Kubernetes Jobs or StatefulSets
   - Use Kubernetes Secrets to manage sensitive data (e.g., API keys)

4. **Evaluation:**

   - Implement model evaluation as Kubernetes CronJobs
   - Store evaluation results in a centralized database (e.g., PostgreSQL deployed on Kubernetes)

5. **Deployment:**

   - Deploy models as Kubernetes Deployments with multiple replicas
   - Use Kubernetes Services for load balancing
   - Implement canary deployments using Kubernetes' traffic splitting features

6. **Monitoring and Maintenance:**

   - Deploy Prometheus and Grafana on Kubernetes for monitoring
   - Use Kubernetes liveness and readiness probes for health checking
   - Implement automated rollbacks using Kubernetes' rollout features

## Best Practices

1. Use lightweight base images (e.g., Alpine Linux) to reduce container size
2. Implement CI/CD pipelines for automating Docker builds and Kubernetes deployments
3. Use Kubernetes namespaces to separate different environments (dev, staging, prod)
4. Implement resource requests and limits in Kubernetes to ensure fair resource allocation
5. Use Kubernetes ConfigMaps and Secrets for managing configuration and sensitive data
6. Implement proper logging in Docker containers and use a centralized logging solution in Kubernetes
7. Regularly update base images and dependencies to address security vulnerabilities
8. Use Kubernetes Network Policies to control traffic between pods

## Citations

- [1] https://blog.stackademic.com/mlops-containerizing-machine-learning-models-with-docker-and-kubernetes-9d01f9741461?gi=596e4072700a
- [2] https://thechief.io/c/cloudplex/tools-start-machine-learning-using-docker-and-kubernetes/
- [3] https://www.iguazio.com/glossary/kubernetes-for-mlops/
- [4] https://www.veritis.com/blog/mlops-best-practices-building-a-robust-machine-learning-pipeline/
- [5] https://github.com/AlexIoannides/kubernetes-mlops
- [6] https://www.datacamp.com/tutorial/tutorial-machine-learning-pipelines-mlops-deployment
- [7] https://overcast.blog/mastering-kubernetes-for-machine-learning-ml-ai-in-2024-26f0cb509d81?gi=7b82660c7c88
- [8] https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning?authuser=2

**NOTE**: We would also like to give credit to **perplexity AI**, which we asked questions to help us obtain the Containerization for MLOps in Medical Imaging with emphasis on Docker and Kubernetes notes: https://www.perplexity.ai/search/you-are-a-software-mlops-engin-P0dJf15zRCiBK.mZ2nzsag#2
