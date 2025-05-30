# 🚀 AWS EMR PySpark Word Count: Scaling from 124MB to 10GB ☁️

![Python](https://img.shields.io/badge/Python-3.7%2B-blue.svg)
![AWS](https://img.shields.io/badge/AWS-EMR%20%7C%20S3%20%7C%20EC2-orange.svg)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-3.5.x-yellow.svg)
![Course](https://img.shields.io/badge/Course-CSE423%20Cloud%20Computing-lightgrey.svg)

This project demonstrates a classic Word Count application implemented using PySpark on Amazon Web Services (AWS) Elastic MapReduce (EMR). It showcases the process of setting up the AWS infrastructure, preparing data (scaling from an initial 124MB text file to a 10GB file), and executing a distributed data processing job.

**Aydın Adnan Menderes University**
**Engineering Faculty**
**Computer Science Engineering Department**
**CSE423 Cloud Computing, Spring 2024/2025**

---

## 📝 Table of Contents

1.  [Project Overview](#project-overview)
2.  [Project Goals](#project-goals)
3.  [Technologies Used](#technologies-used)
4.  [System Architecture](#system-architecture)
5.  [Prerequisites](#prerequisites)
6.  [Setup and Execution Steps](#setup-and-execution-steps)
    *   [Step 1: Create S3 Bucket](#step-1-create-s3-bucket)
    *   [Step 2: Create EMR Cluster](#step-2-create-emr-cluster)
    *   [Step 3: Configure SSH and Security Groups](#step-3-configure-ssh-and-security-groups)
    *   [Step 4: Connect to EMR Master Node](#step-4-connect-to-emr-master-node)
    *   [Step 5: Data Preparation on EMR](#step-5-data-preparation-on-emr)
    *   [Step 6: Code Preparation](#step-6-code-preparation)
    *   [Step 7: Execute the PySpark Job](#step-7-execute-the-pyspark-job)
7.  [The PySpark Script (`word_count.py`)](#the-pyspark-script-word_countpy)
8.  [Expected Output & Results](#expected-output--results)
9.  [Challenges Encountered](#challenges-encountered)
10. [Team Contributions](#team-contributions)
11. [Acknowledgements](#acknowledgements)
12. [References](#references)

---

## 🌟 Project Overview

The primary objective of this project is to perform a word count on a large text dataset using the distributed computing capabilities of Apache Spark, orchestrated by AWS EMR. The process involves:
1.  Setting up an S3 bucket for storing input data, scripts, and output results.
2.  Configuring and launching an AWS EMR cluster with Spark and Hadoop.
3.  Transferring an initial 124MB text file (`leipzig124MB.txt`) to the EMR cluster.
4.  Scaling this file up to approximately 10GB (`large_10gb_file.txt`) directly on the EMR master node.
5.  Uploading the 10GB file to S3.
6.  Executing a PySpark script (`word_count.py`) via `spark-submit` to process the 10GB file from S3 and write the word counts back to S3.

---

## 🎯 Project Goals

*   Demonstrate understanding of AWS S3 for data storage.
*   Illustrate the setup and configuration of an AWS EMR cluster.
*   Showcase distributed data processing using PySpark.
*   Handle large datasets (scaling up to 10GB).
*   Understand the workflow of submitting and monitoring Spark jobs on EMR.
*   Practice connecting to and interacting with EMR nodes via SSH.

---

## 🛠️ Technologies Used

*   **Cloud Platform:** Amazon Web Services (AWS)
    *   **AWS S3 (Simple Storage Service):** For storing input text files, PySpark scripts, and output word counts.
    *   **AWS EMR (Elastic MapReduce):** Managed cluster platform for running big data frameworks like Apache Spark and Hadoop.
    *   **AWS EC2 (Elastic Compute Cloud):** Underlying compute instances for EMR nodes.
*   **Big Data Framework:**
    *   **Apache Spark (PySpark):** Fast and general-purpose cluster computing system, used for the word count logic.
    *   **Apache Hadoop (HDFS & YARN):** Used by EMR for distributed storage (though S3 is primary here) and cluster resource management.
*   **Programming Language:** Python 3.x
*   **Tools:**
    *   **PuTTY:** SSH client for connecting to the EMR master node.
    *   **pscp (PuTTY Secure Copy client):** For transferring files to the EMR master node.
    *   **AWS CLI:** For interacting with AWS services (e.g., copying files to/from S3 from EMR).

---

## 🏗️ System Architecture

A simplified view of the architecture:

```mermaid
graph TD
    A[Local Machine] -- pscp / SSH --> B(EMR Master Node);
    B -- Generates 10GB file --> B;
    B -- aws s3 cp --> C(S3 Bucket: Input Data / Scripts);
    D(EMR Core/Task Nodes) -- Process Data --> D;
    B -- spark-submit --> D;
    C -- Spark Reads --> D;
    D -- Spark Writes --> E(S3 Bucket: Output Results);

    subgraph AWS Cloud
        C
        E
        subgraph EMR Cluster
            B
            D
        end
    end
