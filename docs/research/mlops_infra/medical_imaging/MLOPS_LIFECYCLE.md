# MLOps Lifecycle Notes

## Data Preparation

**TensorFlow & PyTorch:**

- Use `tf.data` (**TensorFlow**) and `torch.utils.data` (**PyTorch**) for efficient data loading pipelines
- Implement data augmentation for medical images (rotation, flipping, contrast adjustment)
- Utilize `tf.io` (**TensorFlow**) and `torchvision` (**PyTorch**) for DICOM file handling

**scikit-learn:**

- Employ sklearn.preprocessing for feature scaling and normalization
- Use sklearn.model_selection for train-test splits and cross-validation

## Model Development

**TensorFlow & PyTorch:**

- Implement CNN architectures (U-Net, ResNet) for medical image segmentation and classification
- Utilize transfer learning with pre-trained models on medical imaging datasets
- Use TensorFlow Keras or PyTorch Lightning for rapid prototyping

**scikit-learn:**

- Implement traditional ML models (Random Forests, SVMs) for non-deep learning tasks
- Use sklearn.pipeline for creating reproducible model workflows

## Training

**TensorFlow & PyTorch:**

- Leverage distributed training with `tf.distribute` (**TensorFlow**) and `torch.distributed` (**PyTorch**)
- Implement mixed precision training for faster computations
- Use TensorBoard or PyTorch's TensorBoard integration for experiment tracking

**scikit-learn:**

- Utilize `sklearn.model_selection.GridSearchCV` for hyperparameter tuning

## Evaluation

**TensorFlow & PyTorch:**

- Implement custom evaluation metrics for medical imaging tasks (Dice coefficient, IoU)
- Use `tf.keras.callbacks` or PyTorch's `torchmetrics` for monitoring during training

**scikit-learn:**

- Employ `sklearn.metrics` for various performance metrics
- Utilize `sklearn.model_selection.cross_val_score` for robust evaluation

## Deployment

**TensorFlow & PyTorch:**

- Use TensorFlow Serving or TorchServe for model serving
- Implement ONNX for model interoperability

**scikit-learn:**

- Use joblib for model serialization and deserialization

## Monitoring

**All tools:**

- Implement logging and monitoring using tools like Prometheus and Grafana
- Set up alerts for model drift and performance degradation

## Load Balancing in MLOps Lifecycle

Load balancing plays a crucial role in the MLOps lifecycle, particularly for medical imaging AI infrastructure. Here's how it impacts the lifecycle with respect to TensorFlow, PyTorch, and scikit-learn:

## Data Preparation

- Distribute data preprocessing tasks across multiple nodes to handle large medical imaging datasets efficiently
- **TensorFlow and PyTorch:** Use `tf.data.Dataset.shard()` or `torch.utils.data.distributed.DistributedSampler` for distributed data loading

## Model Development

- Load balancing is less critical during this phase, but can be used for parallel hyperparameter tuning
- **scikit-learn:** Utilize `joblib` for parallel cross-validation and grid search

## Training

- Critical for distributed training of deep learning models on large medical imaging datasets
- **TensorFlow:** Use `tf.distribute.Strategy` for distributing training across multiple GPUs or TPUs
- **PyTorch:** Employ `torch.nn.parallel.DistributedDataParallel` for multi-GPU training
- **scikit-learn:** Use joblib for parallel model fitting on CPU clusters

## Deployment

- Load balancing is crucial for handling multiple inference requests in production
- **TensorFlow Serving** and **TorchServe:** Configure load balancing for distributing incoming requests across multiple model servers
- Use Kubernetes for orchestrating and scaling model deployment

## Monitoring

- Implement load balancing for log aggregation and metric collection from multiple nodes
- Use tools like Fluentd or Logstash for distributed log collection and processing

Load balancing in the MLOps lifecycle ensures efficient resource utilization, faster processing times, and improved scalability of medical imaging AI infrastructure. It's particularly important when dealing with large-scale datasets and computationally intensive deep learning models common in medical imaging applications

## Citations

[1] https://www.reddit.com/r/learnpython/comments/v9nv0j/should_i_start_with_scikit_learn_tensorflow_or/
[2] https://ritza.co/articles/scikit-learn-vs-tensorflow-vs-pytorch-vs-keras/
[3] https://www.youtube.com/watch?v=LN2NbUkuddA
[4] https://stackoverflow.com/questions/54527439/differences-in-scikit-learn-keras-or-pytorch
[5] https://community.deeplearning.ai/t/scikit-learn-vs-tensorflow/250188
[6] https://www.netguru.com/blog/top-machine-learning-frameworks-compared
[7] https://towardsdatascience.com/scikit-learn-tensorflow-pytorch-keras-but-where-to-begin-9b499e2547d0?gi=263598c3c5be

- **NOTE**: We would also like to give credit to **perplexity AI**, which asked questions to help us obtain the MLOps lifecycle notes: https://www.perplexity.ai/search/you-are-a-software-mlops-engin-P0dJf15zRCiBK.mZ2nzsag
