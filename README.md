# 🖥️ LOG MONITORING WITH ELK STACK

This repository aims to demonstrate log monitoring using an ELK stack. This was accomplished using a virtual machine running Ubuntu. The ELK stack was installed and configured using the documentation provided at https://www.elastic.co/es/. All steps were performed via the command line.

--- 
---
---

## 📇 **CONTENTS**

* [1. INTRODUCTION](#step1)
* [2. ELK STACK DEPLOYMENT](#step2)
* [3. FORMAT DESCRIPTION OF THE SELECTED LOGS. SAMPLE LINE](#step3)

<a name="step1"></a>

## ***1. INTRODUCTION***

The following is a brief explanation of the ELK stack technologies, which are those studied in this module.

- **Logstash**: as a pipeline for processing, transforming, and sending logs.

- **Elasticsearch**: as a storage engine for the information collected from the logs.

- **Kibana**: as a visualization engine for information about the Elasticsearch instance. To prevent authentication, the configuration of the **elasticseash.yml** files (xpack.security.enabled: false) and **kibana.yml** (elasticsearch.username: "" elasticsearch.password: "") has been edited respectively.

<img width="856" height="208" alt="image" src="https://github.com/user-attachments/assets/69bd60ea-3f52-4f72-a732-06f8aa047341" />

<img width="856" height="187" alt="image" src="https://github.com/user-attachments/assets/8f837cf6-9c07-4e33-a4be-f61e50d06000" />

<img width="856" height="229" alt="image" src="https://github.com/user-attachments/assets/e6c0836f-9c67-417b-920a-d7b771bec956" />

---

<a name="step2"></a>

## ***2. ELK STACK DEPLOYMENT***

Once the three tools are installed in their respective directories, running Kibana with the command ´./bin/kibana´ displays the following:

<img width="888" height="220" alt="image" src="https://github.com/user-attachments/assets/25786382-e720-4d0e-9e84-caf4819a2341" />

This indicates that Kibana needs to be configured by accessing http://localhost:5601/?code=508378.
You are prompted to configure Elastic before starting, and for this you need to use the enrollment token provided in the directory where Elasticsearch is installed.

<img width="813" height="805" alt="image" src="https://github.com/user-attachments/assets/6475ac96-6b96-4269-86ac-2e3489767b60" />

This is obtained from the Elasticsearch root folder using the command `bin/elasticsearch-create-enrollment-token --scope kibana`. Note: You need to have an active Elasticsearch instance `./bin/elasticsearch`.

<img width="940" height="235" alt="image" src="https://github.com/user-attachments/assets/d992cd66-7fec-439d-9b3e-978372a74e56" />

Once configured and with the Elasticsearch and Kibana instances started, it is possible to access them from the browser (Firefox) at the address localhost:port.

- **Elasticsearch:** port 9200. Run with ´./bin/elasticsearch´

<img width="903" height="673" alt="image" src="https://github.com/user-attachments/assets/59e0961e-f349-415b-800f-e4235db31211" />

-	**Kibana** : puerto 5610. It was also necessary to add the following line to the Kibana configuration file (kibana.yml): ´xpack.encryptedSavedObjects.encryptionKey: "32characterslongencryptionkey!!!"´. This is to configure the encryption key for encrypted saved objects, which is necessary for alerts, actions, and some plugins to function.

<img width="940" height="511" alt="image" src="https://github.com/user-attachments/assets/9b7d2f2d-7148-4902-9ef5-00aa27645970" />

- **Logstash:** execution with ´./bin/logstash -f mylogstash.conf´, which is the name of the .conf file created for the task and which will be explained later.

---

<a name="step3"></a>

## ***3. FORMAT DESCRIPTION OF THE SELECTED LOGS. SAMPLE LINE.***












