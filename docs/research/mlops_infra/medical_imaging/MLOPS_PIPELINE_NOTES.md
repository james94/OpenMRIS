# MLOps Pipeline Notes

## Data Ingestion and Preparation

**Kubeflow:**

- Use Kubeflow Pipelines to create reusable components for DICOM data loading and preprocessing
- Implement data validation steps using TensorFlow Data Validation (TFDV)

**MLflow:**

- Use MLflow Projects to package data preprocessing scripts
- Track data versions and parameters with MLflow Tracking

**Airflow:**

- Create DAGs for orchestrating data ingestion from PACS systems
- Implement data quality checks and preprocessing tasks as Airflow operators

## Model Development

**Kubeflow:**

- Utilize Kubeflow Notebooks for interactive model development
- Leverage Katib for hyperparameter tuning of medical imaging models

**MLflow:**

- Use MLflow Tracking to log experiments, including hyperparameters and metrics
- Implement MLflow Models for packaging trained models with their dependencies

**Airflow:**

- Create DAGs for orchestrating model training pipelines
- Use Airflow's KubernetesPodOperator for distributed training on GPU clusters

## Training and Evaluation

**Kubeflow:**

- Use Kubeflow Pipelines to create end-to-end training workflows
- Implement distributed training using TensorFlow or PyTorch with Kubeflow's MPIJob

**MLflow:**

- Track model performance metrics using MLflow Tracking
- Compare different model versions using MLflow UI

**Airflow:**

- Schedule periodic model retraining using Airflow's scheduling capabilities
- Implement model evaluation tasks as separate DAGs

## Deployment

**Kubeflow:**

- Use KFServing for model serving, supporting various frameworks (TensorFlow, PyTorch, ONNX)
- Implement canary deployments and A/B testing using Istio integration

**MLflow:**

- Deploy models using MLflow Models, which provides a unified interface for serving
- Utilize MLflow's built-in REST API for model inference

**Airflow:**

- Create DAGs for model deployment and updates
- Use Airflow sensors to trigger deployments based on specific conditions

## Monitoring and Maintenance

**Kubeflow:**

- Implement model monitoring using Kubeflow Pipelines
- Set up alerts for model drift and performance degradation

**MLflow:**

- Use MLflow Tracking to monitor model performance in production
- Implement automated model retraining triggers based on performance metrics

**Airflow:**

- Create DAGs for regular model performance checks and data drift detection
- Implement alerting mechanisms using Airflow's email or Slack operators

## Comparison with Apache NiFi

Apache NiFi is primarily a data integration and flow management tool, while Kubeflow, MLflow, and Airflow are more focused on ML pipeline orchestration. Here's a comparison:

**Strengths of NiFi:**

- Excellent for real-time data ingestion and processing
- Visual interface for designing data flows
- Strong security features and data provenance

**Limitations of NiFi for ML pipelines:**

- Less native support for ML-specific tasks (e.g., distributed training, hyperparameter tuning)
- Not designed for complex workflow orchestration like Airflow

**Complementary use:**

- Use NiFi for data ingestion and initial preprocessing
- Hand off preprocessed data to Kubeflow/MLflow/Airflow for ML-specific tasks

## Load Balancing in MLOps Lifecycle

Load balancing plays a crucial role in the MLOps lifecycle, particularly for medical imaging AI infrastructure. Here's how it impacts the lifecycle with respect to Kubeflow, MLflow, and Airflow:

## Data Ingestion and Preparation

- Distribute data preprocessing tasks across multiple nodes to handle large medical imaging datasets efficiently
- Kubeflow: Use Dask or Apache Spark operators for distributed data processing
- Airflow: Implement parallel task execution using the CeleryExecutor

## Model Development and Training

- Load balance GPU resources for distributed training of deep learning models
- Kubeflow: Utilize MPIJob or PyTorchJob for efficient distribution of training workloads
- MLflow: Integrate with cluster managers like Kubernetes for distributed training

## Model Serving

- Implement load balancing for handling multiple inference requests in production
- Kubeflow: Use KFServing with Istio for intelligent traffic routing and load balancing
- MLflow: Deploy multiple model instances behind a load balancer

## Pipeline Orchestration

- Distribute pipeline execution across multiple worker nodes
- Airflow: Use CeleryExecutor or KubernetesExecutor for scalable task distribution
- Kubeflow Pipelines: Leverage Kubernetes for efficient resource allocation and scaling

Load balancing in the MLOps lifecycle ensures efficient resource utilization, faster processing times, and improved scalability of medical imaging AI infrastructure. It's particularly important when dealing with large-scale datasets and computationally intensive deep learning models common in medical imaging applications [1][2][4].

By implementing these load balancing strategies with Kubeflow, MLflow, and Airflow, you can create robust and scalable MLOps pipelines for medical imaging AI, capable of handling the high computational demands and large datasets typical in healthcare environments.

## Citations

- [1] https://www.veritis.com/blog/mlops-best-practices-building-a-robust-machine-learning-pipeline/
- [2] https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning?authuser=2
- [3] https://ml-ops.org
- [4] https://www.deeplearning.ai/courses/machine-learning-in-production/
- [5] https://www.netguru.com/blog/top-machine-learning-frameworks-compared
- [6] https://www.youtube.com/watch?v=LN2NbUkuddA
- [7] https://community.deeplearning.ai/t/scikit-learn-vs-tensorflow/250188
- [8] https://towardsdatascience.com/scikit-learn-tensorflow-pytorch-keras-but-where-to-begin-9b499e2547d0?gi=263598c3c5be

- **NOTE**: We would also like to give credit to **perplexity AI**, which asked questions to help us obtain the MLOps pipeline notes: https://www.perplexity.ai/search/you-are-a-software-mlops-engin-P0dJf15zRCiBK.mZ2nzsag#1
