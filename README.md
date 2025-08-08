# Splunk Projects Portfolio

Welcome! This repository showcases my hands-on experience using Splunk for cybersecurity and business analytics. Below you will find a summary of my projects. Each project title is a clickable link that will take you to the full details further down this page.

---

## Table of Contents

1.  [**Project 1: Cybersecurity - Investigating Coordinated Failed Logins**](#project-1-investigating-coordinated-failed-logins-cybersecurity)
    * *Analyzed security logs to identify and report on a potential password-spraying attack.*

2.  [**Project 2: Business Analytics - E-commerce Web Traffic Analysis**](#project-2-e-commerce-web-traffic--customer-behavior-analysis-business-analytics)
    * *Analyzed web access logs to derive business intelligence on product popularity and customer geography.*

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
*A screenshot of the final statistics table showing the top targeted user accounts.*
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
*A screenshot of the column chart showing the top 10 customer countries.*
![Splunk Project 2 Column Chart](https://github.com/user-attachments/assets/352faede-5aff-4e71-85a7-ff359b7d6a6f)

[Back to Table of Contents](#table-of-contents)
