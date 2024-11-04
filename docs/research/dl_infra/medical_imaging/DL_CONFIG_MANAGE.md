# Configuration Management for Deep Learning Notes

## YARN (Yet Another Resource Negotiator)

YARN is a cluster management technology used in Hadoop environments. While not directly related to configuration management, it's crucial for resource allocation in distributed deep learning tasks.

**Key Concepts:**

- **Resource Manager:** Central authority for allocating resources to applications
- **Node Manager:** Per-machine framework agent managing containers and resources
- **Application Master:** Per-application entity negotiating resources with Resource Manager

**Best Practices for Deep Learning with YARN:**

1. **Resource Allocation:**

   - Configure `yarn.nodemanager.resource.memory-mb` and `yarn.nodemanager.resource.cpu-vcores` to allocate appropriate resources for deep learning tasks
   - Use `yarn.scheduler.capacity.maximum-am-resource-percent` to control resources for Application Masters

2. **GPU Management:**

   - Enable GPU scheduling with `yarn.nodemanager.resource-plugins=yarn.io/gpu`
   - Set `yarn.nodemanager.resource-plugins.gpu.allowed-gpu-devices` to specify available GPUs

3. **Queue Configuration:**

   - Create separate queues for deep learning jobs using `yarn.scheduler.capacity.<queue-name>.capacity`
   - Set `yarn.scheduler.capacity.<queue-name>.maximum-capacity` to prevent resource hogging

4. **Container Sizing:**

   - Adjust `yarn.scheduler.minimum-allocation-mb` and `yarn.scheduler.maximum-allocation-mb` for appropriate container sizes for deep learning models

5. **Log Aggregation:**

   - Enable with `yarn.log-aggregation-enable=true`
   - Set `yarn.nodemanager.log-dirs` and `yarn.nodemanager.remote-app-log-dir` for log storage

**Example YARN Configuration for Deep Learning:**

```xml
<property>
  <name>yarn.nodemanager.resource.memory-mb</name>
  <value>131072</value> <!-- 128 GB -->
</property>
<property>
  <name>yarn.nodemanager.resource.cpu-vcores</name>
  <value>32</value>
</property>
<property>
  <name>yarn.nodemanager.resource-plugins</name>
  <value>yarn.io/gpu</value>
</property>
<property>
  <name>yarn.scheduler.capacity.root.deeplearning.capacity</name>
  <value>40</value>
</property>
```

## YAML for Configuration Management

YAML is widely used for configuration files in deep learning projects due to its human-readable format and support for complex data structures.

**Best Practices for Deep Learning Configurations:**

1. **Model Architecture:**

   - Define model layers, activation functions, and hyperparameters

2. **Training Configuration:**

   - Specify batch size, learning rate, optimizer, and loss function

3. **Data Pipeline:**

   - Configure data sources, preprocessing steps, and augmentation techniques

4. **Experiment Tracking:**

   - Define metrics to track and logging frequency

5. **Environment Settings:**

   - Specify dependencies, GPU requirements, and distributed training setup

**Example YAML Configuration for Medical Imaging Deep Learning:**

```yaml
model:
  name: "3D_UNet"
  input_shape: [256, 256, 128, 1]
  num_classes: 3
  filters: [32, 64, 128, 256]

training:
  batch_size: 4
  epochs: 100
  optimizer:
    name: "adam"
    learning_rate: 0.001
  loss: "dice_loss"

data:
  train_path: "/path/to/train/data"
  val_path: "/path/to/val/data"
  preprocessing:
    - normalize
    - resize: [256, 256, 128]
  augmentation:
    - random_flip
    - random_rotation: [-10, 10]

logging:
  tensorboard: true
  save_frequency: 5

environment:
  gpu_count: 4
  distributed: true
  framework: "tensorflow"
  version: "2.6.0"
```

## Integrating YARN and YAML in Deep Learning Workflow

1. **Job Submission:**

   - Use YAML to define job configurations
   - Submit jobs to YARN using `yarn jar` command with YAML config file

2. **Resource Allocation:**

   - Specify resource requirements in YAML
   - YARN allocates resources based on these specifications

3. **Distributed Training:**

   - Configure distributed training parameters in YAML
   - YARN manages the distribution of tasks across the cluster

4. **Monitoring:**

   - Use YARN's web interface to monitor job progress
   - Log metrics defined in YAML to YARN's log aggregation system

5. **Version Control:**

   - Store YAML configs in version control systems
   - Use different YAML files for different experiments or model versions

## Medical Imaging Specific Considerations

1. **Data Privacy:**

   - Use YAML to configure data anonymization steps
   - Ensure YARN is configured to handle sensitive medical data securely

2. **Image Preprocessing:**

   - Define preprocessing pipelines for different imaging modalities (MRI, CT, PET, SPECT) in YAML

3. **Model Interpretability:**

   - Configure visualization tools and interpretability methods in YAML

4. **Regulatory Compliance:**

   - Include compliance checks and audit logging in YAML configurations
   - Ensure YARN is set up to maintain proper data access controls

5. **Multi-modal Learning:**

   - Configure fusion of different imaging modalities in YAML
   - Use YARN to allocate resources for complex multi-modal models

## Citations

- [1] https://www.youtube.com/watch?v=dfMi81kNcyY
- [2] https://docs.cloudera.com/runtime/7.2.18/yarn-reference/topics/yarn-config-parameters.html
- [3] https://www.equuscs.com/containerization-deep-learning-ai-workflows/
- [4] https://www.dremio.com/wiki/yarn/
- [5] https://www.davidmcginnis.net/post/a-crash-course-in-yarn-log-aggregation
- [6] https://opensource.com/article/20/9/deep-learning-model-kubernetes
- [7] https://www.datacamp.com/tutorial/containerization-docker-and-kubernetes-for-machine-learning
- [8] https://overcast.blog/mastering-kubernetes-for-machine-learning-ml-ai-in-2024-26f0cb509d81?gi=7b82660c7c88

**NOTE**: We would also like to give credit to **perplexity AI**, which we asked questions to help us obtain the Configuration Management for Deep Learning in Medical Imaging with emphasis on Apache Yarn and YAML notes: https://www.perplexity.ai/search/you-are-a-software-deep-learni-1lgw.qDPSn6E0LIeEmwJKg#3
