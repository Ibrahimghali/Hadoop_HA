---

# Hadoop Cluster with High Availability

### Authors: Ibrahim Ghali & Hamza Zarai

## Introduction
This project demonstrates the implementation of a Hadoop cluster with High Availability (HA). Hadoop is a powerful framework for storing and processing large datasets, but its traditional architecture poses a single point of failure with the NameNode. High Availability ensures continuous operation by enabling automatic failover between Active and Standby NameNodes, providing fault tolerance and improving reliability. This README outlines the steps taken to set up the HA cluster, its architecture, and the configuration details for both ZooKeeper and Hadoop.

---

## Cluster Architecture
Our Hadoop cluster is designed with HA in mind and consists of the following components:

- **ZooKeeper Cluster (3 Nodes):** A distributed coordination service managing leader election and failover for the NameNode.
- **Active NameNode:** The primary NameNode, responsible for managing the HDFS namespace and client interactions.
- **Standby NameNode:** A secondary NameNode that takes over if the Active NameNode fails.
- **DataNodes (2 Nodes):** Nodes that store data blocks. Replication ensures data availability in case of node failure.

### Resource Allocation
The cluster is distributed across two physical machines to improve redundancy and fault tolerance:

- **Machine 1:** Hosts ZooKeeper and the Standby NameNode.
- **Machine 2:** Hosts the Active NameNode and the two DataNodes.

---

## Benefits of High Availability in Hadoop
- **Minimized Downtime:** HA ensures the system remains operational during failures by automatically switching to the Standby NameNode.
- **Increased Fault Tolerance:** The cluster can withstand hardware/software failures without data loss.
- **Improved Scalability:** The architecture allows for easy addition of more DataNodes without affecting availability.

---

## ZooKeeper Configuration
ZooKeeper plays a central role in Hadoop's HA architecture. Below is a summary of the ZooKeeper setup:

1. **Installation:**
   - Install ZooKeeper on each of the three machines.
   - Create a dedicated directory for each instance (e.g., `Zookeeper_node1`).

2. **Configuration (`zoo.cfg`):**
   - Set parameters like `tickTime`, `dataDir`, `clientPort`, `maxClientCnxns`, and define server details for each node.

3. **Unique Server ID:**
   - Each machine has a unique ID stored in `/tmp/zookeeper/myid`.

4. **Starting ZooKeeper:**
   - Use `./zkServer.sh start` to launch ZooKeeper on each machine.

5. **Verification:**
   - Use `echo stat | nc zk_ip 2181` to check ZooKeeper status.
   - Connect to the cluster using `./zkCli.sh`.

---

## Hadoop Configuration for High Availability
To enable HA in Hadoop, several configuration files were updated:

### 1. **HDFS Configuration (`hdfs-site.xml`):**
   - **Replication Factor:** Set to 2 for block replication.
   - **NameNode Directories:** Define paths for metadata and edit logs.
   - **Nameservices:** Logical name for the HA setup.
   - **Failover Configuration:** Automatic failover between Active and Standby NameNodes.
   - **Fencing Methods:** Prevent split-brain scenarios by ensuring only one NameNode is active.

### 2. **Core Configuration (`core-site.xml`):**
   - **Default FileSystem (fs.defaultFS):** Points to the logical name of the HA HDFS cluster.
   - **JournalNode Directories:** Directory paths for storing JournalNode logs.
   - **ZooKeeper Quorum:** IP addresses and ports of ZooKeeper servers for managing failover.

### 3. **YARN Configuration (`yarn-site.xml`):**
   - **ResourceManager HA:** Enables HA for YARN's ResourceManager.
   - **Cluster ID:** Defines active and standby ResourceManagers.
   - **ZooKeeper Quorum:** Manages failover and state recovery for YARN using ZooKeeper.

---

## Repository Branches

This GitHub repository contains **7 branches**, each representing different stages and features of the Hadoop HA setup. You can explore these branches to understand different configurations and aspects of the cluster.

---

## Failover Demonstration

To see how the failover process works when the Active NameNode goes down and the Standby NameNode takes over, check out the following video:

[**Failover Demonstration Video**](https://drive.google.com/file/d/166CURCp4AW2eKYtbxfYLIli0gA9e_w8r/view)

---

## How to Use

1. **Start ZooKeeper Cluster:**  
   On each ZooKeeper node, navigate to the `bin/` directory and run:  
   ```bash
   ./zkServer.sh start
   ```

2. **Start Hadoop Cluster:**  
   Once ZooKeeper is running, start the Active and Standby NameNodes, JournalNodes, and DataNodes by running the Hadoop start script:  
   ```bash
   start-dfs.sh
   start-yarn.sh
   ```

3. **Monitor Cluster:**  
   Use the Hadoop web UI or commands like `hdfs dfsadmin -report` to monitor the health and status of the cluster.

---

## Conclusion
By implementing High Availability in our Hadoop cluster, we significantly enhanced the system's fault tolerance, reliability, and scalability. The ZooKeeper-managed failover mechanism ensures seamless transitions between Active and Standby NameNodes, minimizing downtime during failures.

For detailed configurations and further steps, refer to the configuration files located in this repository.

---

## Contact
For any questions or feedback, feel free to reach out to us:

- **Ibrahim Ghali:** [ibrahim.elghali@outlook.com]
- **Hamza Zarai:** [hamzazarai11@gmail.com]
