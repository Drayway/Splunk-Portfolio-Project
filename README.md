Cybersecurity & Data Analytics Portfolio
Welcome! This repository showcases my hands-on projects using industry-standard tools like Splunk and Wireshark. My goal is to demonstrate practical skills in log analysis, network traffic investigation, and business intelligence.

Each project title in the Table of Contents is a clickable link that will take you to the full details further down this page.

Table of Contents
Project 1: Splunk - Investigating Coordinated Failed Logins (Cybersecurity)

Analyzed security logs to identify and report on a potential password-spraying attack.

Project 2: Splunk - E-commerce Web Traffic Analysis (Business Analytics)

Analyzed web access logs to derive business intelligence on product popularity and customer geography.

Project 3: Wireshark - Malware Traffic Analysis

Analyzed a network packet capture (.pcap) to identify indicators of compromise from a malware infection.

Project 1: Investigating Coordinated Failed Logins (Cybersecurity)
Objective
To analyze raw log data from multiple servers to identify and report on a potential password-spraying attack.

Tools Used
Splunk Enterprise: For data ingestion, searching, and analysis.

SPL (Splunk Processing Language): For querying and data manipulation.

The Process
Data Ingestion: Loaded historical security logs (sourcetype="secure-2") from four servers into Splunk.

Field Extraction: Used the rex command to extract the user field from raw log text, as it was not automatically parsed.

Analysis & Reporting: Wrote an SPL query to count failed login attempts for each user on each host, sorting the results to highlight the most targeted accounts.

Final Splunk Query:
sourcetype="secure-2" "Failed password"
| rex field=_raw "for (?:invalid user )?(?<user>\w+)"
| stats count by user, host
| sort -count

Outcome & Security Insight
The final report revealed a clear pattern of a password-spraying attack. Numerous common accounts (like root, admin, and guest) were being targeted across all servers simultaneously. This analysis provides actionable intelligence for a security team to immediately lock targeted accounts and block attacker IPs.

Visual Evidence
Back to Table of Contents

Project 2: E-commerce Web Traffic & Customer Behavior Analysis (Business Analytics)
Objective
To analyze web access logs for an e-commerce site to derive actionable business intelligence regarding product popularity and customer geography.

Tools Used
Splunk Enterprise

SPL Commands: top, iplocation, stats, sort, head

Visualizations: Column Chart

The Process
Product Analysis: Used the top command on the access_combined_wcookie sourcetype to identify the most frequently viewed and cart-added products.

Geographic Analysis: Leveraged the iplocation and stats commands to count visitors by country and used head to find the top 10.

Visualization: Created a column chart to display the geographic distribution of the top 10 customer countries.

Final Splunk Query (for Geo-Analysis):
sourcetype="access_combined_wcookie"
| iplocation clientip
| stats count by Country
| sort -count
| head 10

Outcome & Business Insight
Delivered a multi-part report identifying the top 10 most popular products and the top 10 customer countries. This directly informs marketing and inventory decisions and allows the business to target specific regions for marketing campaigns.

Visual Evidence
Back to Table of Contents

Project 3: Malware Traffic Analysis
Objective
To analyze a network packet capture file (.pcap) to identify indicators of compromise (IOCs) related to a malware infection.

Tools Used
Wireshark: For packet capture and analysis.

The Process
HTTP Analysis: Opened the .pcap file in Wireshark and filtered for HTTP traffic. Identified the initial point of compromise: a suspicious .zip file (2025-06-13-invoice.zip) downloaded by the user.

DNS Analysis: Filtered for DNS traffic to investigate post-infection activity. Discovered DNS queries to a suspicious, non-standard domain (massfriction.com), identifying the malware's command and control (C2) server.

Conclusion: Correlated the HTTP download with the subsequent DNS query to confirm the infection chain.

Outcome & Security Insight
Successfully identified two critical IOCs:

Malware File: 2025-06-13-invoice.zip

C2 Domain: massfriction.com

This information would allow a security team to create firewall rules to block the C2 domain, preventing further communication, and to deploy antivirus signatures to detect and remove the specific malware file from other infected hosts.

Visual Evidence
Back to Table of Contents
