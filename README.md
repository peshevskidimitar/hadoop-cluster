
# Guide to Running WordCount Job with Hadoop in Docker

## 1. Start Docker Compose with Scaled Data Nodes
```bash
docker compose -f docker-compose.yaml up --scale datanode=3 -d
```
This command initializes a Docker Compose setup with the configuration in `docker-compose.yaml`, scaling the `datanode` service to 3 instances, and runs it in detached mode.

## 2. Access Hadoop Web UI (HDFS)
```plaintext
http://localhost:9870/
```
This URL opens the Hadoop Distributed File System (HDFS) web interface, where you can monitor the status and health of the Hadoop file system.

## 3. Access YARN Resource Manager Web UI
```plaintext
http://localhost:8088/
```
This URL opens the YARN Resource Manager interface to monitor YARN jobs and cluster resources.

## 4. Copy a Local File into the Docker Container
```bash
docker cp input.txt namenode:/opt/hadoop
```
Copies `input.txt` from the local machine into the `namenode` container at the specified path (`/opt/hadoop`).

## 5. Access the Namenode Container’s Command Line
```bash
docker exec -it namenode /bin/bash
```
Executes an interactive terminal (`/bin/bash`) within the `namenode` container, allowing you to run commands directly inside it.

## 6. Create Directory in HDFS
```bash
hdfs dfs -mkdir -p /usr/root/input
```
Creates the directory `/usr/root/input` in HDFS, using the `-p` option to create parent directories as needed.

## 7. Upload File to HDFS
```bash
hdfs dfs -put input.txt /usr/root/input
```
Transfers `input.txt` from the local filesystem into the HDFS directory `/usr/root/input`.

## 8. List Files in HDFS Directory
```bash
hdfs dfs -ls /usr/root/input
```
Displays the contents of the `/usr/root/input` directory in HDFS.

## 9. Run Hadoop MapReduce Job (WordCount)
```bash
hadoop jar /opt/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar wordcount /usr/root/input /usr/root/output
```
Executes a Hadoop MapReduce job using the WordCount example, taking `/usr/root/input` as input and outputting results to `/usr/root/output`.

## 10. List Files in HDFS Output Directory
```bash
hdfs dfs -ls /usr/root/output
```
Shows the contents of the `/usr/root/output` directory in HDFS, where the results of the WordCount job are stored.

## 11. View WordCount Output
```bash
hdfs dfs -cat /usr/root/output/part-r-00000
```
Displays the content of the `part-r-00000` file, which contains the WordCount job's output.

## 12. Download Output from HDFS to Local Directory
```bash
hdfs dfs -get /usr/root/output/part-r-00000 /opt/hadoop
```
Copies the `part-r-00000` file from HDFS to the local directory `/opt/hadoop`.

## 13. Remove Output Directory in HDFS
```bash
hdfs dfs -rm -r /usr/root/output
```
Deletes the `/usr/root/output` directory in HDFS, freeing up space for future jobs.

## 14. Exit the Docker Container Shell
```plaintext
CTRL + D
```
Press `CTRL + D` to exit the `namenode` container’s interactive shell.

## 15. Stop Docker Compose
```bash
docker compose -f docker-compose.yaml down
```
Stops all services defined in the `docker-compose.yaml` file and removes containers, networks, and other resources.
