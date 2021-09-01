# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior

### Network Topology


The following machines were identified on the network:
- Host
  - **Operating System**: Windows
  - **Purpose**: Contains VM's to use
  - **IP Address**:192.168.1.1

- Kali
  - **Operating System**: Kali Linux
  - **Purpose**: Attacking Machine
  - **IP Address**:192.168.1.90

- Elk
  - **Operating System**: Linux
  - **Purpose**: SIEM that watches over the webserver
  - **IP Address**:192.168.1.100

- Capstone
  - **Operating System**: Linux
  - **Purpose**: Holds a Capstone Apache Server
  - **IP Address**:192.168.1.105

- Target 1
  - **Operating System**: Linux
  - **Purpose**: First Target Machine to Attack
  - **IP Address**:192.168.1.110

- Target 2
  - **Operating System**: Linux
  - **Purpose**: Second Target Machine to Attack
  - **IP Address**:192.168.1.115

### Description of Targets

The target of this attack was: `Target 1` 192.168.1.110.

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

Excessive HTTP Errors is implemented as follows:
  - **Metric**: WHEN count() GROUPED OVER top 5 'http.response.status_code'
  - **Threshold**: IS ABOVE 400 FOR THE LAST 5 minutes
  - **Vulnerability Mitigated**: Possible HTTP flood attacks, possible Denial of 		Service attacks
  - **Reliability**: This alert like the HTTP Request Monitor alert, it triggers tons 		of false positives and its hard to tell which is a real, and whats false. Like 	the HTTP Request alert we need to edit and change the threshold to a larger 	value so we can come to a more concrete idea of if there really was a attack 	instead of looking through around 24 pages of data.

#### HTTP Request Size Monitor
HTTP Request Size Monitor is implemented as follows:
  - **Metric**: WHEN sum() of http.request.bytes OVER all documents
  - **Threshold**:  IS ABOVE 3500 FOR THE LAST 1 minute
  - **Vulnerability Mitigated**: Possible HTTP request smuggling attacks,possible     		attempts at enumeration
  - **Reliability**: This alert has its positives but it seems to producing a ton of 		false positives and creating a lot of triggers compared to the CPU Usage 		alert. May have     to edit the threshold to make it a little more strict.

#### CPU Usage Monitor
CPU Usage Monitor is implemented as follows:
  - **Metric**: WHEN max() OF system.process.cpu.total.pct OVER all documents
  - **Threshold**: IS ABOVE 0.5 FOR THE LAST 5 minutes
  - **Vulnerability Mitigated**: Possible Denial of Serivce attack to the webserver
  - **Reliability**:  This alert is very good, only triggered two alerts and seems reliable and not triggered too many false positives. In my opinion it has a high reliability for     future use.
