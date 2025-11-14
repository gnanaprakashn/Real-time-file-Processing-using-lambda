

# 🚀 Real-Time File Processing using AWS Lambda

This project implements a **serverless, event-driven pipeline** for processing files in **real-time** using **AWS S3, SQS, Lambda, and DynamoDB**.
A Windows service monitors a folder for incoming files and automatically uploads them to an S3 bucket.
Each upload triggers an automated workflow that cleans, transforms, and stores the processed data.

---

## 📌 **Architecture Overview**

<img src="real time file processing.png" width="850"/>

---

## 🧩 **How It Works**

### **1️⃣ Windows Folder Watcher (Source Layer)**

* A lightweight **Windows background service** monitors a folder.
* Whenever a new file arrives, it automatically uploads the file to **S3 (Raw Zone)**.

### **2️⃣ S3 Bucket (Storage Layer)**

* Stores incoming raw files.
* Triggers **S3 Event Notifications** with file metadata (key, path, size, timestamp).

### **3️⃣ SQS Queue (Event Layer)**

* Receives metadata from S3.
* Delivers messages reliably and asynchronously to Lambda.

### **4️⃣ AWS Lambda (Processing Layer)**

Lambda is triggered for **each incoming SQS message**.
It performs:

* 📥 Fetch object from S3
* 🔄 Parse & transform the file
* 📤 Write cleaned records into **DynamoDB**

### **5️⃣ DynamoDB (Database Layer)**

* Stores the final structured data
* Supports fast lookup & scalable ingestion

### **6️⃣ IAM + CloudWatch (Security & Monitoring)**

* IAM policies allow secure communication between services
* CloudWatch tracks logs, metrics
* SNS sends error/alert notifications

---

## 🗂️ **Project Structure**

```
📦 Real-Time File Processing
├── lambda code.py          # AWS Lambda processing script
├── real time file processing.png   # Architecture diagram
└── README.md               # Documentation
```

---

## 🔧 **Technologies Used**

| Service             | Purpose                      |
| ------------------- | ---------------------------- |
| 🖥️ Windows Service | Folder monitoring            |
| 🪣 Amazon S3        | Raw file storage             |
| 📩 Amazon SQS       | Event messaging              |
| λ AWS Lambda        | File processing              |
| 🗃️ DynamoDB        | Processed data storage       |
| 🔐 IAM              | Permissions & access control |
| 📊 CloudWatch       | Logs & monitoring            |

---

## 📝 **Lambda Code Summary**

Your Lambda script handles:

* Downloading the object from S3
* Parsing file contents
* Structuring the data
* Writing batches to DynamoDB
* Logging, error handling, and retries

---

## ⚙️ **Deployment Steps**

1. Create S3 bucket → enable event notifications
2. Create SQS queue → link with S3 event
3. Create Lambda function → attach SQS trigger
4. Add IAM permissions:

   * `S3:GetObject`
   * `SQS:ReceiveMessage`
   * `DynamoDB:PutItem`
   * `Lambda:Invoke`
5. Deploy the Windows Folder Watcher service
6. Upload sample files → verify pipeline flow

---

## 🧪 **Testing the Pipeline**

✔️ Drop a file into the monitored Windows folder
✔️ Check S3 raw zone
✔️ Confirm SQS receives metadata
✔️ Lambda logs show processing steps
✔️ DynamoDB table gets new records

---

## 📢 **Future Enhancements**

* Add file validation & schema checks
* Support CSV, JSON, XML, Parquet
* Add Athena for analytics
* Create SNS notifications for success metrics
* Implement dead-letter queue for failures

---

## 🙌 **Contributors**

👤 **Gnana Prakash N**
* Data Engineer | AWS | Python | Spark | Automation*

