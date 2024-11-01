# Deep Learning Lifecycle Notes

## Data Preparation

**scikit-learn:**

- Use for data preprocessing, feature selection, and scaling
- Implement train-test splits and cross-validation
- Utilize `StandardScaler`, `MinMaxScaler` for normalization

**TensorFlow and PyTorch:**

- Create custom data loaders and datasets
- Implement data augmentation techniques for medical images
- Use `tf.data` **(TensorFlow)** or `torch.utils.data` **(PyTorch)** for efficient data pipelines

## Model Development

**TensorFlow:**

- Use Keras API for quick prototyping
- Implement custom layers with `tf.keras.layers.Layer`
- Utilize transfer learning with pre-trained models from TensorFlow Hub

**PyTorch:**

- Define models using `nn.Module`
- Implement custom loss functions
- Use `torchvision` for pre-trained models and transfer learning

## Training

**TensorFlow:**

- Use `tf.keras.Model.fit()` for simple training loops
- Implement custom training loops with `tf.GradientTape`
- Utilize tf.distribute for multi-GPU training

**PyTorch:**

- Implement training loops manually
- Use `torch.optim` for optimizers
- Leverage `torch.nn.parallel` for distributed training

## Evaluation and Validation

**scikit-learn:**

- Use `metrics` module for evaluation metrics (e.g., accuracy, F1-score)
- Implement cross-validation with `model_selection` module

**TensorFlow and PyTorch:**

- Create custom evaluation metrics
- Implement validation loops
- Use TensorBoard (TensorFlow) or PyTorch Lightning for visualization

## Model Optimization

**TensorFlow:**

- Use `tf.keras.tuner` for hyperparameter tuning
- Implement pruning and quantization with TensorFlow Model Optimization Toolkit

**PyTorch:**

- Use `torch.optim.lr_scheduler` for learning rate scheduling
- Implement early stopping and model checkpointing

## Deployment

**TensorFlow:**

- Use TensorFlow Serving for model deployment
- Implement TensorFlow Lite for mobile and edge devices

**PyTorch:**

- Use TorchScript for model serialization
- Implement ONNX for cross-platform deployment

## Medical Imaging Specific Considerations

- Implement 2D and 3D convolutional neural networks for image analysis
- Use attention mechanisms for focusing on specific regions of interest
- Implement segmentation models (e.g., U-Net) for organ or lesion delineation
- Utilize transfer learning from models pre-trained on large medical imaging datasets

## Best Practices

- **Version control:** Use Git for code and DVC for data versioning
- **Containerization:** Implement Docker for reproducible environments
- **CI/CD:** Set up automated testing and deployment pipelines
- **Documentation:** Use tools like Sphinx for comprehensive documentation
- **Experiment tracking:** Implement MLflow or Weights & Biases for experiment management

## Healthcare Compliance

- Ensure HIPAA compliance in data handling and model deployment
- Implement robust data anonymization techniques
- Design systems with auditability and traceability in mind
- Consider FDA regulations for AI/ML in medical devices


## Citations:

[1] https://www.reddit.com/r/learnpython/comments/v9nv0j/should_i_start_with_scikit_learn_tensorflow_or/
[2] https://ritza.co/articles/scikit-learn-vs-tensorflow-vs-pytorch-vs-keras/
[3] https://stackoverflow.com/questions/54527439/differences-in-scikit-learn-keras-or-pytorch
[4] https://community.deeplearning.ai/t/scikit-learn-vs-tensorflow/250188
[5] https://www.youtube.com/watch?v=LN2NbUkuddA
[6] https://towardsdatascience.com/scikit-learn-tensorflow-pytorch-keras-but-where-to-begin-9b499e2547d0?gi=263598c3c5be
[7] https://www.netguru.com/blog/top-machine-learning-frameworks-compared

- **NOTE**: We would also like to give credit to **perplexity AI**, which asked questions to help us obtain the Deep Learning lifecycle notes: https://www.perplexity.ai/search/you-are-a-software-deep-learni-1lgw.qDPSn6E0LIeEmwJKg