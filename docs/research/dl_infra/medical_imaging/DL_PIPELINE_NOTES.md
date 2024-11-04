# Deep Learning Pipeline Notes

## Kubeflow

**Key Features**:

  - Kubernetes-native platform for ML workflows
  - End-to-end orchestration of ML pipelines
  - Built-in experiment tracking and model serving

**Pipeline Development**:

  - Use Kubeflow Pipelines SDK to define workflows
  - Create reusable components for each step in your pipeline
  - Leverage pre-built components for common tasks

**Deployment**:

  - Deploy on Kubernetes clusters (on-premises or cloud)
  - Utilize Kubeflow Serving for model deployment
  - Integrate with Istio for advanced networking and security

**Medical Imaging Considerations**:

  - Use Kubeflow Pipelines for preprocessing large imaging datasets
  - Implement distributed training for 3D medical image models
  - Deploy models for real-time inference on medical imaging data

## MLflow

**Key Features**:

  - Experiment tracking and model versioning
  - Model packaging and deployment
  - Central model registry

**Pipeline Development**:

  - Use MLflow Projects to define reproducible workflows
  - Track experiments and parameters with MLflow Tracking
  - Package models using MLflow Models for consistent deployment

**Deployment**:

  - Deploy models to various serving platforms (e.g., Azure ML, AWS SageMaker)
  - Use MLflow Model Registry for model lifecycle management
  - Implement CI/CD pipelines for model updates

**Medical Imaging Considerations**:

  - Track experiments for different medical imaging modalities
  - Version control large medical imaging models
  - Manage model artifacts for various imaging AI applications

## Airflow

**Key Features**:

  - General-purpose workflow orchestration
  - Extensible with custom operators and hooks
  - Rich UI for monitoring and managing workflows

**Pipeline Development**:

  - Define DAGs (Directed Acyclic Graphs) for workflow structure
  - Use PythonOperators for custom Python code execution
  - Implement BashOperators for shell script execution

**Deployment**:

  - Deploy on various platforms (on-premises, cloud, or managed services)
  - Scale horizontally with Celery or Kubernetes executors
  - Integrate with external systems using hooks and operators

**Medical Imaging Considerations**:

  - Orchestrate end-to-end medical imaging pipelines
  - Schedule periodic model retraining on new imaging data
  - Implement data quality checks for incoming medical images

## Comparison with Apache NiFi

Apache NiFi is a data flow management tool, while Kubeflow, MLflow, and Airflow are more focused on ML workflow orchestration. Here's a comparison:

| Feature | NiFi | Kubeflow | MLflow | Airflow |
|---------|------|----------|--------|---------|
| Primary Focus | Data ingestion and routing | ML workflows on Kubernetes | ML experiment tracking and model management | General workflow orchestration |
| UI-based Design | Yes | Limited | No | Yes |
| Scalability | Horizontal | Kubernetes-based | Limited | Horizontal |
| ML-specific Features | Limited | Extensive | Extensive | Limited |
| Data Provenance | Strong | Limited | Limited | Limited |
| Learning Curve | Moderate | Steep | Moderate | Moderate |

**When to use NiFi vs. ML pipeline tools**:

- Use NiFi when:
  - You need robust data ingestion and routing capabilities
  - Real-time data flow management is crucial
  - You require strong data provenance and lineage tracking

- Use ML pipeline tools (Kubeflow, MLflow, Airflow) when:
  - You need end-to-end ML workflow orchestration
  - Experiment tracking and model versioning are essential
  - You require seamless integration with ML frameworks and cloud services

For medical imaging AI infrastructure, you might consider using NiFi for initial data ingestion and preprocessing, then handing off to one of the ML pipeline tools for model training, evaluation, and deployment. This combination can provide a powerful end-to-end solution for managing complex medical imaging AI workflows.


## Citations

- [1] https://stackoverflow.com/questions/59046257/what-are-the-differences-between-airflow-and-kubeflow-pipeline
- [2] https://valohai.com/blog/kubeflow-vs-mlflow/
- [3] https://www.run.ai/guides/machine-learning-operations/mlflow-vs-kubeflow
- [4] https://towardsdatascience.com/airflow-vs-luigi-vs-argo-vs-mlflow-vs-kubeflow-b3785dd1ed0c?gi=e714720fd454
- [5] https://www.linkedin.com/pulse/airflow-vs-kubeflow-mlflow-whats-difference-jack-molloy
- [6] https://community.deeplearning.ai/t/tfx-vs-kubeflow/556671
- [7] https://www.reddit.com/r/mlops/comments/1evza42/mlflow_vs_kubeflow/
- [8] https://www.netguru.com/blog/top-machine-learning-frameworks-compared

- **NOTE**: We would also like to give credit to **perplexity AI**, which asked questions to help us obtain the Deep Learning Pipeline (Kubeflow, MLFlow, AirFlow, NiFi) notes: https://www.perplexity.ai/search/you-are-a-software-deep-learni-1lgw.qDPSn6E0LIeEmwJKg#1
