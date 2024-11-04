# MLOps Lifecycle with MapR Storage Notes

1. Data Ingestion:

   - Use MapR NFS Gateway for direct file access:

     ```
     mount -t nfs -o vers=3 <mapr-cluster>:/mapr /mapr
     ```

   - Leverage MapR Stream for real-time data ingestion:

     ```java
     MapRStreams producer = new MapRStreams();
     producer.publish("medical_images_stream", imageData);
     ```

2. Data Preprocessing:

   - Use MapR-XD for distributed storage and processing:

     ```
     hadoop jar preprocessing.jar -input /mapr/medical_images -output /mapr/preprocessed_images
     ```

3. Feature Extraction:

   - Utilize MapR-DB for storing extracted features:

     ```java
     Document features = MapRDB.newDocument()
         .set("patient_id", patientId)
         .set("features", extractedFeatures);
     table.insertOrReplace(features);
     ```

4. Model Training:

   - Use MapR-XD for distributed training data access:

     ```python
     from mapr.ojai.storage.ConnectionFactory import ConnectionFactory
     connection = ConnectionFactory.get_connection(connection_str)
     training_data = connection.get_store('/training_data').find()
     ```

5. Model Evaluation:

   - Store evaluation results in MapR-DB:

     ```java
     Document evaluation = MapRDB.newDocument()
         .set("model_version", version)
         .set("accuracy", accuracy)
         .set("date", new Date());
     evaluationTable.insertOrReplace(evaluation);
     ```

6. Model Deployment:

   - Use MapR Container for Kubernetes (MCK) for deployment:

     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: medical-ai-model
     spec:
       containers:
       - name: model-server
         image: medical-ai-model:v1
         volumeMounts:
         - name: mapr-volume
           mountPath: /mapr
     ```

7. Monitoring:

   - Use MapR Monitoring for cluster and application monitoring:

     ```
     maprcli dashboard info -json
     ```

## Load Balancing: MapR Storage vs Hadoop

1. MapR Storage:

   - Uses a distributed metadata architecture, eliminating single points of failure[1].
   - Automatic data balancing across nodes without manual intervention.
   - Supports multi-tenancy with volume-based resource allocation.

2. Hadoop (HDFS):

   - Relies on NameNode for metadata management, which can be a bottleneck[1].
   - Requires manual rebalancing using the `hdfs balancer` tool.
   - Limited support for multi-tenancy.

Load balancing in MapR:

```
maprcli volume balance -cluster <cluster_name> -localvolumeonly true
```

Load balancing in Hadoop:

```
hdfs balancer -threshold 10
```

## Setting up Big Data Storage: MapR vs Hadoop

### MapR Storage Setup:

1. Install MapR Converged Data Platform:

   ```
   sudo rpm --import https://package.mapr.com/releases/pub/maprgpg.key
   sudo yum install mapr-fileserver mapr-cldb mapr-webserver
   ```

2. Configure the cluster:

   ```
   sudo /opt/mapr/server/configure.sh -C <cldb-host> -Z <zk-host> -L /mapr
   ```

3. Create a volume for medical imaging data:

   ```
   maprcli volume create -name medical_images -path /medical_images
   ```

4. Set up MapR-XD for distributed storage:

   ```
   maprcli config save -values {"mfs.enable.xd":"1"}
   ```

### Hadoop (HDFS) Setup:

1. Install Hadoop:

   ```
   wget https://downloads.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz
   tar -xzf hadoop-3.3.1.tar.gz
   ```

2. Configure core-site.xml:

   ```xml
   <property>
     <name>fs.defaultFS</name>
     <value>hdfs://localhost:9000</value>
   </property>
   ```

3. Configure hdfs-site.xml:

   ```xml
   <property>
     <name>dfs.replication</name>
     <value>3</value>
   </property>
   ```

4. Format the NameNode:

   ```
   hdfs namenode -format
   ```

5. Start HDFS:

   ```
   start-dfs.sh
   ```

## Key differences

- MapR provides a unified storage system (MapR-XD) that supports multiple data access methods (HDFS, NFS, POSIX)[2].
- MapR offers better performance for random read/write operations compared to HDFS[2].
- MapR provides built-in disaster recovery and high availability features without additional configuration[2].

## Citations

- [1] https://stackoverflow.com/questions/32482850/difference-between-typical-hadoop-architecture-and-mapr-architecture
- [2] https://softwareengineeringdaily.com/2018/11/09/converged-data-platform-unifying-streaming-data-using-mapr/
- [3] https://www.ijeast.com/papers/119-122,Tesma408,IJEAST.pdf
- [4] https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9568311/
- [5] https://www.integrate.io/blog/apache-spark-vs-hadoop-mapreduce/
- [6] https://resources.experfy.com/bigdata-cloud/cloudera-vs-hortonworks-comparing-hadoop-distributions/
- [7] https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/yarn-service/Configurations.html
- [8] https://docs.cloudera.com/runtime/7.2.18/yarn-reference/topics/yarn-config-parameters.html

**NOTE**: We would also like to give credit to **perplexity AI**, which we asked questions to help us obtain the MapR Storage for MLOps in Medical Imaging AI: https://www.perplexity.ai/search/you-are-a-software-mlops-engin-P0dJf15zRCiBK.mZ2nzsag#6
