# MLOps for Medical Imaging AI Notes

We'll walk through comprehensive notes for MLOps in medical imaging AI, focusing on the data science tools: Kubernetes (Rancher), Apache Drill, Apache Livy (Spark), MapR, Jupyter, R (RStudio), GPU Setup & Management, and Harvest for Resource Monitoring.


### 1. Kubernetes (Rancher) for Orchestration

- Setup:

  ```
  rke up --config rancher-cluster.yml
  ```

- Deploy Rancher:

  ```
  helm install rancher rancher-latest/rancher \
    --namespace cattle-system \
    --set hostname=rancher.my.org
  ```

- Create ML workload:

  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: mri-classifier
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: mri-classifier
    template:
      metadata:
        labels:
          app: mri-classifier
      spec:
        containers:
        - name: mri-classifier
          image: my-registry/mri-classifier:v1
          resources:
            limits:
              nvidia.com/gpu: 1
  ```

### 2. Apache Drill for Data Querying

- Connect to Drill:

  ```python
  from pydrill.client import PyDrill
  drill = PyDrill(host='localhost', port=8047)
  ```

- Query DICOM metadata:

  ```sql
  SELECT
    t.`PatientName`,
    t.`StudyDate`,
    t.`Modality`
  FROM dfs.`/path/to/dicom/files` t
  WHERE t.`Modality` = 'MRI'
  ```

### 3. Apache Livy for Spark Job Submission

- Submit Spark job:

  ```python
  import json
  import requests

  host = "http://livy-server:8998"
  data = {
      'className': 'org.apache.spark.examples.SparkPi',
      'executorMemory': '1g',
      'executorCores': 1,
      'numExecutors': 1
  }
  headers = {'Content-Type': 'application/json'}
  r = requests.post(host + '/batches', data=json.dumps(data), headers=headers)
  ```

### 4. MapR for Distributed Storage

- Mount MapR volume:

  ```bash
  sudo mount -t nfs -o vers=3,nolock,hard,intr,proto=tcp maprfs:/ /mapr
  ```

- Store DICOM files:

  ```python
  import pydicom
  from maprfs import MapRFile

  ds = pydicom.dcmread("patient001.dcm")
  with MapRFile("/mapr/medical_cluster/dicom/patient001.dcm", "w") as f:
      f.write(ds.to_json())
  ```

### 5. Jupyter Notebooks for Interactive Development

- Launch secure Jupyter server:

  ```bash
  jupyter notebook --certfile=mycert.pem --keyfile mykey.key
  ```

- Load medical images:

  ```python
  import nibabel as nib
  import matplotlib.pyplot as plt

  img = nib.load('brain_mri.nii.gz')
  plt.imshow(img.get_fdata()[:,:,100], cmap='gray')
  plt.show()
  ```

### 6. R (RStudio) for Statistical Analysis

- Install required packages:

  ```r
  install.packages(c("oro.dicom", "oro.nifti", "neuRosim"))
  ```

- Analyze MRI data:

  ```r
  library(oro.nifti)
  mri_data <- readNIfTI("brain_mri.nii.gz")
  hist(mri_data)
  ```

### 7. GPU Setup and Management

- Check GPU status:

  ```bash
  nvidia-smi
  ```

- Set up GPU in TensorFlow:

  ```python
  import tensorflow as tf
  gpus = tf.config.experimental.list_physical_devices('GPU')
  tf.config.experimental.set_memory_growth(gpus[0], True)
  ```

### 8. Harvest for Resource Monitoring

- Configure Harvest for Kubernetes:

  ```yaml
  # harvest.yml
  exporters:
    prometheus:
      port: 12990
  collectors:
    - name: k8s
      interval: 10s
  ```

- Query GPU metrics:

  ```promql
  sum(nvidia_gpu_memory_used_bytes) by (node)
  ```

### MLOps Lifecycle Integration

1. Data Ingestion:

   - Use MapR for storing raw DICOM/NIfTI files
   - Use Apache Drill to query metadata and filter relevant studies

2. Data Preprocessing:

   - Deploy preprocessing jobs on Kubernetes using Rancher
   - Use GPU-accelerated containers for image normalization and augmentation

3. Model Development:

   - Develop models in Jupyter Notebooks
   - Use R for statistical analysis of model performance

4. Training:

   - Submit distributed training jobs via Apache Livy
   - Monitor GPU utilization with Harvest

5. Evaluation:

   - Use R for advanced statistical analysis of model results
   - Visualize results in Jupyter Notebooks

6. Deployment:

   - Package models in containers and deploy via Rancher
   - Use Kubernetes for scaling and load balancing

7. Monitoring:

   - Use Harvest to monitor resource utilization and model performance
   - Set up alerts for GPU memory usage and inference latency

These notes cover the key aspects of using these data science tools in an MLOps lifecycle for medical imaging AI. It demonstrates proficiency in containerization, distributed computing, interactive development, GPU utilization, and monitoring - all crucial for building and maintaining large-scale medical AI systems.

## Citations

- [1] https://ranchermanager.docs.rancher.com/how-to-guides/new-user-guides/kubernetes-clusters-in-rancher-setup
- [2] https://www.iguazio.com/glossary/kubernetes-for-mlops/
- [3] https://home.mlops.community/public/blogs/rke2-kubernetes-cluster-bare-metal-environment
- [4] https://komodor.com/learn/kubernetes-rancher-the-basics-and-a-quick-tutorial/
- [5] https://gcloud.devoteam.com/ebook/the-machine-learning-on-google-cloud-workbook/the-basics-of-mlops/
- [6] https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning?authuser=2
- [7] https://mlops.community/rke2-kubernetes-cluster-bare-metal-environment/
- [8] https://developer.harness.io/docs/continuous-integration/development-guides/mlops/e2e-mlops-tutorial/

**NOTE**: We would also like to give credit to **perplexity AI**, which we asked questions to help us obtain the Data Science Tools for MLOps in Medical Imaging AI notes: https://www.perplexity.ai/search/you-are-a-software-mlops-engin-P0dJf15zRCiBK.mZ2nzsag#10
