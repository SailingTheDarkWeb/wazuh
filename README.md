# Wazuh

Wazuh is a free and open source security platform that unifies XDR and SIEM capabilities.

## Wazuh Components

The Wazuh solution is based on the Wazuh agent, which is deployed on the monitored endpoints, and on three central components: the Wazuh server, the Wazuh indexer, and the Wazuh dashboard.

![Wazuh Components](https://github.com/user-attachments/assets/8bc64e39-c721-4744-80aa-9ac0c781cef1)
<p align = 'center'><b>Figure 1: Components of Wazuh</b>

### 1. Wazuh Agent

Wazuh agents are installed on endpoints such as laptops, desktops, servers, cloud instances, or virtual machines. They provide threat prevention, detection, and response capabilities.

#### a. Agent Architecture

The Wazuh agent has a modular architecture. Each component oversees its own tasks, including monitoring the file system, reading log messages, collecting inventory data, scanning the system configuration, and looking for malware.

![Wazuh Agent](https://github.com/user-attachments/assets/d4aa0aab-0f9e-49d3-a487-00ad6b95facf)
<p align = 'center'><b>Figure 2: Wazuh Agent Architecture and Modules</b></p>

#### b. Agent Modules

All agent modules are configurable and perform different security tasks. This modular architecture allows you to enable or disable each component according to your security needs.

List of modules:
  1. **Log Collector** - This agent component can read flat log files and Windows events, collecting operating system and application log messages.
  2. **Command Execution** - Agents run authorized commands periodically, collecting their output and reporting it back to the Wazuh server for further analysis.
  3. **File Integrity Monitoring (FIM)** - This module monitors the file system, reporting when files are created, deleted, or modified. It keeps track of changes in file attributes, permissions, ownership, and                                              content. When an event occurs, it captures who, what, and when details in real time.
  4. **Security Configuration Assessment (SCA)** - This component provides continuous configuration assessment, utilizing out-of-the-box checks based on the Center of Internet Security (CIS) benchmarks.
  5. **System Inventory** - This agent module periodically runs scans, collecting inventory data such as operating system version, network interfaces, running processes, installed applications, and a list of open                             ports.
  6. **Malware Detection** - Using a non-signature-based approach, this component is capable of detecting anomalies and the possible presence of rootkits.
  7. **Active Response** - This module runs automatic actions when threats are detected, triggering responses to block a network connection, stop a running process, or delete a malicious file.
  8. **Container Security** - This agent module is integrated with the Docker Engine API to monitor changes in a containerized environment.
  9. **Cloud Security** - This component monitors cloud providers such as Amazon Web Services, Microsoft Azure, or Google GCP. It natively communicates with their APIs.

#### c. Agent Daemon

List of features under Agent Daemon are:
  1. **Modules Management**
  2. **Data Encryption**
  3. **Data Flow Control**
  4. **Remote Configuration**

### 2. Wazuh Server

The Wazuh server component analyzes the data received from the agents, triggering alerts when threats or anomalies are detected. It is also used to manage the agent’s configuration remotely and monitor their status.

![Wazuh Server](https://github.com/user-attachments/assets/afc89cbc-e32b-406c-8558-9179a257848d)
<p align = 'center'><b>Figure 3: Wazuh Server Architecture and components</b></p>

#### a. Server Components

1. **Agent Enrollment Service** - It is used to enroll new agents. This service provides and distributes unique authentication keys to each agent.
2. **Agent Connection Service** - This service receives data from the agents. It uses the keys shared by the enrollment service to validate each agent identity and encrypt the communications between the Wazuh                                       agent and the Wazuh server.
3. **Analysis Engine** - This is the server component that performs the data analysis. It uses decoders to identify the type of information being processed (Windows events, SSH logs, web server logs, and others).                          These decoders also extract relevant data elements from the log messages, such as source IP address, event ID, or username. Then, by using rules, the engine identifies specific patterns 
                         in the decoded events that could trigger alerts and possibly even call for automated countermeasures.
4. **Wazuh RESTful API** - This service provides an interface to interact with the Wazuh infrastructure. It is used to manage configuration settings of agents and servers, monitor the infrastructure status and                               overall health, manage and edit Wazuh decoders and rules, and query about the state of the monitored endpoints.
5. **Wazuh Cluster Daemon** - This service is used to scale Wazuh servers horizontally, deploying them as a cluster. This kind of configuration, combined with a network load balancer, provides high availability                                 and load balancing.
6. **Filebeat** - It is used to send events and alerts to the Wazuh indexer. It reads the output of the Wazuh analysis engine and ships events in real time.

### 3. Wazuh Indexer

The Wazuh indexer is a highly scalable, full-text search and analytics engine. This Wazuh central component indexes and stores alerts generated by the Wazuh server and provides near real-time data search and analytics capabilities.

The Wazuh indexer stores data as JSON documents. Each document correlates a set of keys, field names or properties, with their corresponding values which can be strings, numbers, booleans, dates, arrays of values, geolocations, or other types of data.

Different indices:
  1. **wazuh-alerts** - Stores alerts generated by the Wazuh server.
  2. **wazuh-archives** - Stores all events (archive data) received by the Wazuh server, whether or not they trip a rule.
  3. **wazuh-monitoring** - Stores data related to the Wazuh agent status over time.
  4. **wazuh-statistics** - Stores data related to the Wazuh server performance. It is used by the web interface to represent the performance statistics.

  


