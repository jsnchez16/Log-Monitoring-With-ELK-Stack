# 🖥️ LOG MONITORING WITH ELK STACK

This repository aims to demonstrate log monitoring using an ELK stack. This was accomplished using a virtual machine running Ubuntu. The ELK stack was installed and configured using the documentation provided at https://www.elastic.co/es/. All steps were performed via the command line.

--- 
---
---

## 📇 **CONTENTS**

* [1. INTRODUCTION](#step1)
* [2. ELK STACK DEPLOYMENT](#step2)

<a name="step1"></a>

## ***1. INTRODUCTION***

The following is a brief explanation of the ELK stack technologies, which are those studied in this module.

- **Logstash**: as a pipeline for processing, transforming, and sending logs.

- **Elasticsearch**: as a storage engine for the information collected from the logs.

- **Kibana**: as a visualization engine for information about the Elasticsearch instance. To prevent authentication, the configuration of the elasticseash.yml files (xpack.security.enabled: false) and kibana.yml (elasticsearch.username: "" elasticsearch.password: "") has been edited respectively.

<img width="856" height="208" alt="image" src="https://github.com/user-attachments/assets/69bd60ea-3f52-4f72-a732-06f8aa047341" />

<img width="856" height="187" alt="image" src="https://github.com/user-attachments/assets/8f837cf6-9c07-4e33-a4be-f61e50d06000" />

<img width="843" height="226" alt="image" src="https://github.com/user-attachments/assets/004995aa-94fa-47a8-b1ae-448cfc9e2da0" />

---

<a name="step2"></a>

## ***2. ELK STACK DEPLOYMENT***
