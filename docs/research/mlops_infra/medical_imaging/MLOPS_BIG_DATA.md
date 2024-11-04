# MLOps Big Data - Hadoop and Hive Notes

Here are notes on deep learning design and development tailored to the MLOps lifecycle, focusing on Big Data tools like Hadoop and Hive for medical imaging AI infrastructure.


## Hadoop Configuration

**Core-site.xml:**

```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://namenode:9000</value>
  </property>
</configuration>
```

**HDFS-site.xml:**

```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>3</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/path/to/namenode</value>
  </property>
</configuration>
```

**Yarn-site.xml:**

```xml
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
</configuration>
```

## Hive Configuration

**Hive-site.xml:**

```xml
<configuration>
  <property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/user/hive/warehouse</value>
  </property>
  <property>
    <name>hive.execution.engine</name>
    <value>tez</value>
  </property>
</configuration>
```

## MLOps Lifecycle Integration

1. **Data Ingestion:**

   - Use Hadoop's DistCp for efficient data transfer:

     ```
     hadoop distcp /local/path hdfs:///medical_images
     ```

   - Create Hive external tables for medical image metadata:

     ```sql
     CREATE EXTERNAL TABLE mri_scans (
       patient_id STRING,
       scan_date DATE,
       image_path STRING
     )
     STORED AS PARQUET
     LOCATION '/medical_data/mri_scans';
     ```

2. **Data Preprocessing:**

   - Use Hadoop Streaming for parallel image preprocessing:

     ```
     hadoop jar hadoop-streaming.jar \
       -input /medical_images \
       -output /preprocessed_images \
       -mapper preprocess_image.py \
       -reducer NONE
     ```

3. **Feature Extraction:**

   - Implement feature extraction using Hive UDFs:

     ```sql
     CREATE FUNCTION extract_features AS 'com.hospital.ml.FeatureExtractor'
     USING JAR 'hdfs:///jars/feature_extractor.jar';

     INSERT OVERWRITE TABLE image_features
     SELECT patient_id, extract_features(image_data)
     FROM raw_images;
     ```

4. **Model Training:**

   - Use Hadoop YARN for distributed training:

     ```
     yarn jar deep_learning_job.jar \
       -D mapreduce.job.queuename=ml_training \
       -input /image_features \
       -output /trained_model
     ```

5. **Model Evaluation:**

   - Implement evaluation metrics using Hive queries:

     ```sql
     SELECT
       AVG(accuracy) as mean_accuracy,
       STDDEV(accuracy) as std_accuracy
     FROM model_predictions
     JOIN ground_truth ON predictions.id = ground_truth.id;
     ```

6. **Model Deployment:**

   - Store model artifacts in HDFS:

     ```
     hdfs dfs -put /local/model /models/medical_imaging_v1
     ```

7. **Monitoring:**

   - Use Hive for aggregating and analyzing model performance metrics:

     ```sql
     CREATE TABLE model_metrics (
       model_version STRING,
       timestamp TIMESTAMP,
       accuracy FLOAT,
       latency INT
     )
     PARTITIONED BY (date STRING);
     ```

## Load Balancing with Hadoop and Hive

1. **HDFS Load Balancing:**

   - Enable HDFS balancer:

     ```
     hdfs balancer -threshold 10
     ```

   - Monitor data distribution:

     ```
     hdfs dfsadmin -report
     ```

2. **YARN Resource Management:**

   - Configure YARN capacity scheduler for fair resource allocation:

     ```xml
     <property>
       <name>yarn.scheduler.capacity.root.queues</name>
       <value>default,ml_training</value>
     </property>
     ```

3. **Hive Query Load Balancing:**

   - Use Hive's cost-based optimizer:

     ```sql
     SET hive.cbo.enable=true;
     ```

   - Implement partitioning for large tables:

     ```sql
     ALTER TABLE model_metrics ADD PARTITION (date='2024-10-31');
     ```

4. **Data Skew Handling:**

   - Use Hive's skew join optimization:

     ```sql
     SET hive.optimize.skewjoin=true;
     ```

## Setting up Big Data Storage for Hadoop

1. **HDFS Architecture:**

   - Configure NameNode and DataNodes:

     ```
     hdfs namenode -format
     start-dfs.sh
     ```

2. **Storage Planning:**

   - Estimate storage needs:

     ```
     Total Storage = (Raw Data Size * Replication Factor) * Growth Factor
     ```

   - Plan for at least 3x replication for fault tolerance

3. **Hardware Considerations:**

   - Use commodity hardware for cost-effectiveness
   - Ensure sufficient network bandwidth between nodes

4. **Data Organization:**

   - Implement a logical directory structure:

     ```
     /medical_data
       /raw_images
       /preprocessed
       /features
       /models
     ```

5. **Data Ingestion Pipeline:**

   - Set up Flume or Kafka for real-time data ingestion
   - Use DistCp for bulk data transfers

6. **Security:**

   - Enable Kerberos authentication:

     ```xml
     <property>
       <name>hadoop.security.authentication</name>
       <value>kerberos</value>
     </property>
     ```

   - Implement HDFS encryption zones for sensitive medical data

7. **Backup and Recovery:**

   - Set up HDFS snapshots:

     ```
     hdfs dfsadmin -allowSnapshot /medical_data
     hdfs dfs -createSnapshot /medical_data daily_backup
     ```

   - Implement off-site replication for disaster recovery

8. **Monitoring and Maintenance:**

   - Use Hadoop's built-in monitoring tools:

     ```
     hdfs dfsadmin -report
     yarn node -list -all
     ```

   - Implement regular health checks and maintenance schedules

By following these notes, you'll be well-prepared to discuss how to leverage Hadoop and Hive in the MLOps lifecycle for medical imaging AI infrastructure. This approach demonstrates your understanding of big data tools in the context of healthcare applications, addressing key aspects like data storage, processing, and model management at scale.

## Citations

- [1] https://www.projectpro.io/article/mlops-lifecycle/885
- [2] https://www.youtube.com/watch?v=9JIiGfQ1ps8
- [3] https://www.bitstrapped.com/blog/mlops-lifecycle-explained-by-stages
- [4] https://www.veritis.com/blog/mlops-best-practices-building-a-robust-machine-learning-pipeline/
- [5] https://www.iguazio.com/glossary/kubernetes-for-mlops/
- [6] https://www.datacamp.com/tutorial/tutorial-machine-learning-pipelines-mlops-deployment
- [7] https://nioyatech.com/mlops-tools/
- [8] https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning?authuser=2

**NOTE**: We would also like to give credit to **perplexity AI**, which we asked questions to help us obtain the MLOps Big Data Tools for Deep Learning in Medical Imaging with emphasis on Hadoop and Hive notes: https://www.perplexity.ai/search/you-are-a-software-mlops-engin-P0dJf15zRCiBK.mZ2nzsag#5
