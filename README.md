
# WordCount-Using-MapReduce-Hadoop

## Project Overview
This project implements a Word Count program using Hadoop MapReduce in Java. The application processes a text dataset, counts the number of occurrences of each word, and outputs the results. It is built using Maven and designed to run on a Hadoop cluster.

## Approach and Implementation
- **Mapper (WordMapper.java):**
  - Reads each line from the input file.
  - Tokenizes the line into words.
  - Emits `(word, 1)` pairs for each valid word.

- **Reducer (WordReducer.java):**
  - Receives words from mappers grouped by key.
  - Aggregates occurrences by summing up the values.
  - Outputs `(word, count)` pairs.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn install
```

### 3. **Move JAR File to Shared Folder**

Move the generated JAR file to a shared folder for easy access:

```bash
mv target/*.jar shared-folder/input/code/
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp /workspaces/hands-on-3-mapreduce-word-count-using-map-reduce-BhanuThanniru/shared-folder/input/data/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/dataset
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/dataset
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command:

```bash
hadoop jar WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/dataset/input.txt /output
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ shared-folder/output/
    ```
## Challenges faced
    While executing the code, though the code is correct, the output is not being generated. It is being stopped at map 17% reduce 0% and a pop up is shown that the resources are utilized exceeding the limit.

## Sample Input
    Hello world
    Hello Hadoop
    Hadoop is powerful
    Hadoop is used for big data

## Sample Output
    Hadoop 3
    Hello 2
    is 2
    used 1
    for 1
    big 1
    data 1
    powerful 1
    world 1