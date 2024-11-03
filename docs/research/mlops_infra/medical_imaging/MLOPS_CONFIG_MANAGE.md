# MLOps Configuration Management Cheatsheet

## Yarn Configuration

**Project Setup:**

- Use `.yarnrc.yml` at the project root for Yarn configuration
- Enable strict mode:

  ```yaml
  enableStrictSsl: true
  ```

**Dependency Management:**

- Lock file: `yarn.lock`
- Install dependencies: `yarn install`
- Add new dependency: `yarn add <package>`
- Remove dependency: `yarn remove <package>`

**Scripts:**

- Define custom scripts in `package.json`:

  ```json
  "scripts": {
    "preprocess": "python preprocess_dicom.py",
    "train": "python train_model.py",
    "evaluate": "python evaluate_model.py"
  }
  ```

- Run scripts: `yarn run <script-name>`

**Workspaces:**

- Set up monorepo for modular ML pipeline:

  ```yaml
  # package.json
  "workspaces": [
    "packages/*"
  ]
  ```

**Environment Variables:**

- Use `.env` files for environment-specific configs
- Access in scripts: `process.env.VARIABLE_NAME`

## YAML Configuration

**Model Configuration:**

```yaml
model:
  name: unet
  input_shape: [256, 256, 1]
  num_classes: 3
  learning_rate: 0.001
  batch_size: 32
```

**Data Pipeline:**

```yaml
data:
  train_path: /path/to/train_data
  val_path: /path/to/val_data
  augmentations:
    - random_flip
    - random_rotation
  preprocessing:
    normalization: z_score
    resize: [256, 256]
```

**Training Configuration:**

```yaml
training:
  epochs: 100
  early_stopping:
    patience: 10
    monitor: val_loss
  model_checkpoint:
    save_best_only: true
    monitor: val_dice_coef
```

**Evaluation Metrics:**

```yaml
evaluation:
  metrics:
    - dice_coefficient
    - iou
    - precision
    - recall
  threshold: 0.5
```

**Deployment:**

```yaml
deployment:
  model_format: onnx
  target_devices:
    - cpu
    - gpu
  serving:
    framework: triton
    max_batch_size: 8
```

## MLOps Lifecycle Integration

1. **Data Preparation:**

   - Use Yarn scripts for data preprocessing tasks
   - Define data pipeline configs in YAML

2. **Model Development:**

   - Store model architecture and hyperparameters in YAML
   - Use Yarn workspaces for modular development

3. **Training:**

   - Configure training parameters in YAML
   - Run training scripts with Yarn

4. **Evaluation:**

   - Define evaluation metrics and thresholds in YAML
   - Execute evaluation scripts via Yarn

5. **Deployment:**

   - Specify deployment configs in YAML
   - Use Yarn scripts for model conversion and deployment

6. **Monitoring:**

   - Configure monitoring thresholds in YAML
   - Set up Yarn scripts for periodic model performance checks

## Best Practices

1. Version control your YAML configs and `yarn.lock` file
2. Use environment-specific YAML files (e.g., `config.dev.yml`, `config.prod.yml`)
3. Implement a config validation step in your CI/CD pipeline
4. Use Yarn's workspaces feature for managing complex ML projects
5. Leverage Yarn's caching capabilities for faster builds and deployments
6. Use YAML anchors and aliases for DRY (Don't Repeat Yourself) configurations
7. Implement secret management for sensitive information (e.g., API keys)
8. Use Yarn's `--frozen-lockfile` flag in CI/CD to ensure reproducibility

## Example: Integrating Yarn and YAML in MLOps Workflow

1. **Project Structure:**

   ```
   medical-ai-project/
   ├── .yarnrc.yml
   ├── package.json
   ├── yarn.lock
   ├── config/
   │   ├── model.yml
   │   ├── data.yml
   │   └── training.yml
   ├── src/
   │   ├── preprocess/
   │   ├── train/
   │   ├── evaluate/
   │   └── deploy/
   └── scripts/
       ├── run_pipeline.sh
       └── monitor_performance.sh
   ```

2. **Yarn Scripts (package.json):**

   ```json
   {
     "scripts": {
       "preprocess": "python src/preprocess/main.py",
       "train": "python src/train/main.py",
       "evaluate": "python src/evaluate/main.py",
       "deploy": "python src/deploy/main.py",
       "pipeline": "yarn preprocess && yarn train && yarn evaluate && yarn deploy"
     }
   }
   ```

3. **YAML Configuration (config/model.yml):**

   ```yaml
   model:
     name: densenet121
     input_shape: [512, 512, 3]
     num_classes: 5
     transfer_learning:
       base_model: imagenet
       trainable_layers: 30
   ```

4. **Running the Pipeline:**

   ```bash
   yarn run pipeline
   ```

These notes provide a comprehensive overview of using Yarn and YAML for configuration management in medical imaging AI MLOps. It covers key aspects of the MLOps lifecycle and demonstrates how to integrate these tools effectively in your workflow.

## Citations

- [1] https://yarnpkg.com/configuration/yarnrc
- [2] http://yarn.dask.org/en/latest/configuration.html
- [3] https://support.huaweicloud.com/eu/usermanual-codeci/codeci_ug_1069.html
- [4] https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/yarn-service/Configurations.html
- [5] https://www.w3resource.com/yarn/yarn-check-and-yarn-config-cli-commands.php
- [6] https://docs.aws.amazon.com/codeartifact/latest/ug/npm-yarn.html
- [7] https://github.com/backstage/backstage/issues/14023
- [8] https://docs.cloudera.com/runtime/7.2.18/yarn-reference/topics/yarn-config-parameters.html
