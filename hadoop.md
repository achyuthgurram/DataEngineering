
**Hadoop:** It is a framework that allows distributed processing of large datasets across commodity computers using simple programming models.

Hadoop - Doug cutting

Hadoop is not just a single system. It contains multiple tools for injestion, multiple tools for storing the data, multiple tools for data processing, multiple tools for data analysis, multiple tools for data visualization, multiple tools for data exploration.

![Hadoop eco system](../images/hadoop_ecosystem.png)

Hadoop brings processing engine close to the data instead of bringing data close to processing engine where we are processing petabytes of data. processing engine is like a jar file which is an executable code and we bring this to the data instead of bringing data to the server where jar is executed.


Hadoop core components

1. HDFS -> storage
2. YARN -> resources (RAM + CPU)
3. Map reduce/ spark -> processing engine, business logic

Just like an application running on your machine requires CPU, memory and disk, hadoop has YARN and HDFS to provide those resources and Map reduce is the processing or application using which we execute our code.

Spark is very quick compared to HDFS/ Map reduce.

HDFS id distributed storage system that provides access to data across hadoop.

HDFS stores files in number of blocks. Default setting of HDFS is it replicates file blocks by 3 times and every file block is of size 128MB.

HDFS command to see the list of file blocks associated with a file and their locations. fsck in below command stands for file system check.

```bash
hdfs fsck /path/to/file -files -blocks -locations
```

All configuration regarding the replication factor, where to place the file blocks is present in /opt/hadoop/etc/hadoop/hdfs-site.xml. Location can change depending on installation.

sample properties
1. dfs.replication
2. dfs.blocksize

replication factor is defined on cluster level. So it effects all the files on the cluster

HDFS master - name node
slave - data node

1 master 3 slave is 4 node cluster

1 master will act like master for all hadoop services like HDFS, HBASE, YARN etc... There is no separate master for each of these services.

There is also a backup of master node.

Master keeps checking if the slave nodes are available or not using health checks.

Name nodes stores metadata of cluster i.e. which datanode has what block.

Data nodes store actual data.

standby name node should always be in sync with active name node

zookeeper always monitors active and standby name node and if active name node doesn't respond to health check, then it broadcast message to all data nodes making the stand by name node the active name node.

![HDFS architecture](../images/hdfs_architecture.png)


Hadoop versions 1.x, 2.x, 3.x

no one is using 1.x

1.x - max 200 data nodes, only 1 name node, SPOF

2.x - max 4000 data nodes, 1 active name node, 1 standby name node. Realtime communication between active and standby name node.

3.x - 3 masters and infinite data nodes

2 data structures that maintain metadata inside name node.

1. Edit logs -> Transaction logs of recent meta data changes
2. FS Image -> file system image -> snapshot of full file system metadata. It basically is the folder structure of all the files across all data nodes. It is stored in memory.

metadata = edit logs + FS Image

active name node will only need to share edit logs with standby node. Sharing the entire snapshot is not required and not a good idea.

Below diagram suggests how metadata sync happens between the active and standby name nodes

![Metadata sync between nodes](../images/metadata_sync_between_namenodes.png)

There are disadvantages with shared storage because it is SPOF and also writing to disk is slow. Ideal approach is Quorum based storage.

Quorum based storage writes the edit logs to multiple journal nodes and uses the quorum to generate a read consistency.

When ever there is new data in journal node, it generates an event for standby node so it is aware of the new data. Stand by node can perform only read operation on journal node. Only when it becomes active, it will have write permission.

Quorum journal manager is used for highest consistency between active and standby nodes.

Quorum is the minimum number of nodes required for a decision, typically N/2 + 1 in distributed systems to ensure consistency and availability.

cloud engineering = data engineering + ML + Devops

Jupyter hub is IDE for python and scala

HUE is GUI for shell for hadoop

create a file with 250MB size

```bash
dd if=/dev/zero of=sample_file.bin bs=1M count=250
```

Write mechanism of new data to HDFS

![HDFS client write data to storage](../images/Data-Write-Mechanism-in-HDFS.gif)


Read mechanism of data in HDFS

![HDFS client write data to storage](../images/Data-Read-Mechanism-in-HDFS.gif)



```bash
hdfs dfs -ls /user/axgurr

hdfs dfs -mkdir /user/axgurr/test_dir_1

hdfs dfs -put sample_file.bin /user/axgurr1/test_dir_1

hdfs dfs -chmod 777 /user/axgurr1/*

hdfs dfs -cat /user/axgurr1/test_dir_1/sample_file.bin
```

HDFS uses rack awareness algorithm to place the file blocks on data nodes

Data engineers build pipelines to move data from databases to data warehouses.
On data warehouses we do more reads compared to databases where we do more writes.
Data warehouses store 100TB to petabytes and even bigger amount of data.

OLTP data stored in schema or tables, OLAP we follow dimension tables, fact tables or dimension modeling concepts.

ex: redshift, big query



**Links**

Google drive books link:

https://drive.google.com/drive/u/3/folders/1eOrjAkr5uyegYJ0x9qcXNZZ8VYeV2ubU

HDFS default configuration

https://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml

HDFS commands

https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html

Java code to write to HDFS

https://github.com/saagie/example-java-read-and-write-from-hdfs/blob/master/src/main/java/io/saagie/example/hdfs/Main.java