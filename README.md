# Cybersecurity & Data Analytics Portfolio

Welcome! This repository showcases my hands-on projects using industry-standard tools like Splunk and Wireshark. My goal is to demonstrate practical skills in log analysis, network traffic investigation, and business intelligence.

Each project title in the Table of Contents is a clickable link that will take you to the full details further down this page.

---

## Table of Contents

1.  [**Project 1: Splunk - Investigating Coordinated Failed Logins (Cybersecurity)**](#project-1-investigating-coordinated-failed-logins-cybersecurity)
    * *Analyzed security logs to identify and report on a potential password-spraying attack.*

2.  [**Project 2: Splunk - E-commerce Web Traffic Analysis (Business Analytics)**](#project-2-e-commerce-web-traffic--customer-behavior-analysis-business-analytics)
    * *Analyzed web access logs to derive business intelligence on product popularity and customer geography.*

3.  [**Project 3: Wireshark - Malware Traffic Analysis**](#project-3-malware-traffic-analysis)
    * *Analyzed a network packet capture (`.pcap`) to identify indicators of compromise from a malware infection.*

---
---

## **Project 1: Investigating Coordinated Failed Logins (Cybersecurity)**

### **Objective**
To analyze raw log data from multiple servers to identify and report on a potential password-spraying attack.

### **Tools Used**
* **Splunk Enterprise:** For data ingestion, searching, and analysis.
* **SPL (Splunk Processing Language):** For querying and data manipulation.

### **The Process**
1.  **Data Ingestion:** Loaded historical security logs (`sourcetype="secure-2"`) from four servers into Splunk.
2.  **Field Extraction:** Used the `rex` command to extract the `user` field from raw log text, as it was not automatically parsed.
3.  **Analysis & Reporting:** Wrote an SPL query to count failed login attempts for each user on each host, sorting the results to highlight the most targeted accounts.

#### **Final Splunk Query:**
```spl
sourcetype="secure-2" "Failed password"
| rex field=_raw "for (?:invalid user )?(?<user>\w+)"
| stats count by user, host
| sort -count
```

### **Outcome & Security Insight**
The final report revealed a clear pattern of a password-spraying attack. Numerous common accounts (like `root`, `admin`, and `guest`) were being targeted across all servers simultaneously. This analysis provides actionable intelligence for a security team to immediately lock targeted accounts and block attacker IPs.

### **Visual Evidence**
![Splunk Final Result](https://github.com/user-attachments/assets/21c1ae50-d3e0-484a-855d-b5b384535e30)

[Back to Table of Contents](#table-of-contents)

---
---

## **Project 2: E-commerce Web Traffic & Customer Behavior Analysis (Business Analytics)**

### **Objective**
To analyze web access logs for an e-commerce site to derive actionable business intelligence regarding product popularity and customer geography.

### **Tools Used**
* **Splunk Enterprise**
* **SPL Commands:** `top`, `iplocation`, `stats`, `sort`, `head`
* **Visualizations:** Column Chart

### **The Process**
1.  **Product Analysis:** Used the `top` command on the `access_combined_wcookie` sourcetype to identify the most frequently viewed and cart-added products.
2.  **Geographic Analysis:** Leveraged the `iplocation` and `stats` commands to count visitors by country and used `head` to find the top 10.
3.  **Visualization:** Created a column chart to display the geographic distribution of the top 10 customer countries.

#### **Final Splunk Query (for Geo-Analysis):**
```spl
sourcetype="access_combined_wcookie"
| iplocation clientip
| stats count by Country
| sort -count
| head 10
```

### **Outcome & Business Insight**
Delivered a multi-part report identifying the top 10 most popular products and the top 10 customer countries. This directly informs marketing and inventory decisions and allows the business to target specific regions for marketing campaigns.

### **Visual Evidence**
![Splunk Project 2 Column Chart](https://github.com/user-attachments/assets/352faede-5aff-4e71-85a7-ff359b7d6a6f)

[Back to Table of Contents](#table-of-contents)

---
---

## **Project 3: Malware Traffic Analysis**

### **Objective**
To analyze a network packet capture file (`.pcap`) to identify indicators of compromise (IOCs) related to a malware infection.

### **Tools Used**
* **Wireshark:** For packet capture and analysis.

### **The Process**
1.  **HTTP Analysis:** Opened the `.pcap` file in Wireshark and filtered for HTTP traffic. Identified the initial point of compromise: a suspicious `.zip` file (`2025-06-13-invoice.zip`) downloaded by the user.
2.  **DNS Analysis:** Filtered for DNS traffic to investigate post-infection activity. Discovered DNS queries to a suspicious, non-standard domain (`massfriction.com`), identifying the malware's command and control (C2) server.
3.  **Conclusion:** Correlated the HTTP download with the subsequent DNS query to confirm the infection chain.

### **Outcome & Security Insight**
Successfully identified two critical IOCs:
* **Malware File:** `2025-06-13-invoice.zip`
* **C2 Domain:** `massfriction.com`

This information would allow a security team to create firewall rules to block the C2 domain, preventing further communication, and to deploy antivirus signatures to detect and remove the specific malware file from other infected hosts.

### **Visual Evidence**
![Wireshark DNS Analysis](https://github.com/user-attachments/assets/8b17e672-1daf-4ef1-9642-15355bf05b2c)

[Back to Table of Contents](#table-of-contents)
