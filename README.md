# 🖥️ LOG MONITORING WITH ELK STACK

This repository aims to demonstrate log monitoring using an ELK stack in a lab environment accomplished using a virtual machine running Ubuntu. The ELK stack was installed and configured using the documentation provided at https://www.elastic.co/es/. All steps were performed via the command line.

--- 
---
---

## 📇 **CONTENTS**

* [1. INTRODUCTION](#step1)
* [2. ELK STACK DEPLOYMENT](#step2)
* [3. FORMAT DESCRIPTION OF THE SELECTED LOGS. SAMPLE LINE](#step3)
* [4. LOGSTASH CONFIGURATION](#step4)
* [5. KIBANA DASHBOARD. VISUALIZATIONS](#step5)  
* [6. SOME POSSIBLE CONFIGURATION PROBLEMS](#step6)
---

<a name="step1"></a>

## ***1. INTRODUCTION***

The following is a brief explanation of the ELK stack technologies.

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

-	**Kibana** : port 5610. It was also necessary to add the following line to the Kibana configuration file (kibana.yml): ´xpack.encryptedSavedObjects.encryptionKey: "32characterslongencryptionkey!!!"´. This is to configure the encryption key for encrypted saved objects, which is necessary for alerts, actions, and some plugins to function.

<img width="940" height="511" alt="image" src="https://github.com/user-attachments/assets/9b7d2f2d-7148-4902-9ef5-00aa27645970" />

- **Logstash:** execution with ´./bin/logstash -f mylogstash.conf´, which is the name of the .conf file created for the task and which will be explained later.

---

<a name="step3"></a>

## ***3. FORMAT DESCRIPTION OF THE SELECTED LOGS. SAMPLE LINE***

For this task, a simulated log file was used, following this model line:

´<timestamp> user=<username> file=<file_path> action=<action> status=<status> category=<category>´

Applied to the example, this looks like this:

´2025-05-22 15:01:00 user=jgomez file=/etc/passwd action=read status=success category=security´

This provides:

o timestamp
o user: username
o file: path to the accessed file
o action: action performed on the accessed file
o status: result of the action
o category: type of event

<img width="688" height="672" alt="image" src="https://github.com/user-attachments/assets/873da9b8-362a-458f-8769-56175ddd523b" />

---

<a name="step4"></a>

## ***4. LOGSTASH CONFIGURATION***

To process the information from the previous logs, a configuration file named milogstash.conf has been defined in the Logstash folder.

First, **file{}** as **Input Plugin** is used to collect data from the aforementioned log file. The fields used are:

- **path:** indicating the path where the log file is located.

- **start_position:** ensures that the file is read from the beginning.

- **sincedb_path:** ensures that the file is read from the beginning each time it is run.

<img width="769" height="347" alt="image" src="https://github.com/user-attachments/assets/de384d05-1ead-4b2a-9dc5-b0ecb9dc26f7" />

When the Logstash file plugin is executed, it reads each line of the file and converts each line into an event. The content of each line is saved in the **message** field, which is automatically created by Logstash when the file plugin is executed.

Following the Input plugin, the **grok{}** and **date{}** **Filter plugins** are used to extract the log fields in a structured manner.

<img width="940" height="366" alt="image" src="https://github.com/user-attachments/assets/fcaf8aa5-2ef5-4e55-85be-ad7d7ac03847" />

For grok, the **match** block is used to determine that the desired data is in the message field mentioned earlier. This block extracts the values ​​from the specified fields, following the log pattern, so that they are subsequently available as individual fields in Elasticsearch and Kibana.

This extracts the information from the timestamp, user, file, action, status, and category fields.

For date, which converts date and time strings into digital formats, the **match** and **target** fields are used. The timestamp field is collected in the specified format, and this is then designated as the target in Kibana.

Finally, the **Elasticsearch{}** is used as **Output plugin**. The hosts field specifies the localhost address and port where the Elasticsearch instance (cluster) is running, as well as the index, where the indexed information should be stored. **stdout{}** is used for debugging.

<img width="702" height="381" alt="image" src="https://github.com/user-attachments/assets/fe2a6160-9a00-43ea-9e52-4ff988d53bcd" />

---

<a name="step5"></a>

## ***5. KIBANA DASHBOARD. VISUALIZATIONS***

Finally, we have the visualization of the structured data collected by Logstash in Kibana. As mentioned earlier, Kibana is the visualization engine for the Elasticsearch instances where the data we want to work with is located.

When you access Kibana, with the data index already created (it was created automatically by Logstash), you can see a summary of the information collected from the logs in the Discover tab. There will be as many Documents as there are lines of processed logs.

<img width="940" height="291" alt="image" src="https://github.com/user-attachments/assets/78116c54-0d3b-4491-9ce9-676ebf3cb1d0" />

The left tab contains the Available fields section, which lists all the fields found, including those defined in the Logstash conf file. Dragging these fields onto the Dashboard allows you to organize the information for each log.

<img width="940" height="297" alt="image" src="https://github.com/user-attachments/assets/65b6df43-d96c-43f9-85c0-6407b0ea1c1f" />

It's important to note that we need to work with the desired Data View. In this case, the one created is SimLogs_DataView; when creating it, you choose the reference index that contains the information you want to work with.

<img width="417" height="266" alt="image" src="https://github.com/user-attachments/assets/508ee62e-b927-4d21-8482-3351f2c4e2e7" />

To display more elaborate visualizations, access the Visualize Library tab and create the desired visualizations.

<img width="940" height="372" alt="image" src="https://github.com/user-attachments/assets/f075fca5-e12f-4521-ae56-b493a76bf77e" />

The following are some visualizations for analyzing the collected logs.

- **Actions per user:** graph of the number of actions performed by users.

<img width="1032" height="514" alt="image" src="https://github.com/user-attachments/assets/1b947528-108a-4971-b4f0-313b0c99b490" />

- **Files accessed and action performed:** pie chart showing the percentage of files accessed and the action performed on them.

<img width="1041" height="444" alt="image" src="https://github.com/user-attachments/assets/70df098e-049d-4786-bccc-8f86ba50ccbc" />

- **Percentage of failures/successes in the actions:** mosaic with the percentage of actions and the percentage of success/failure.

<img width="968" height="450" alt="image" src="https://github.com/user-attachments/assets/3bd100d4-8223-4101-918a-fc4719773fc4" />

❗❗Integrating data visualization into a dashboard isn't just a matter of including the data as it appears: we can use explanatory text, formatted with Markdown, to help the reader understand what's being displayed. It might also be necessary to manually adjust the number of buckets in a pie chart if, for example, there are six items but only five slices are shown. These dashboards can also be saved for sharing and reference on public displays.

---

<a name="step6"></a>

## ***6. SOME POSSIBLE CONFIGURATION PROBLEMS***

**1. PORT 5601 IN USE KIBANA ALREADY IN USE (USE SUDO)**

- Verify that Kibana is running:

´ps aux | grep kibana´

- See which process is using port 5601:

´sudo lsof -i :5601´

- Close the Kibana instance:

´kill PID (THE ONE OBTAINED IN THE PREVIOUS COMMAND)´

2. Why isn't anything showing up in Kibana?

Logstash only processes new lines appended to the end of the file (like a "tail -f" command), unless you force it with `start_position => "beginning"` and `sincedb_path => "/dev/null"`.

👉 this only works if the file is modified after Logstash starts. If the file is unchanged, Logstash won't process it.
