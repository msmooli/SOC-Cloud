# Building a SOC + Honeynet in Azure (Live Traffic)
<img width="2064" height="1204" alt="Screenshot 2025-10-22 at 10 20 53 AM" src="https://github.com/user-attachments/assets/06bebfec-5be0-4b61-92e2-b2305a524293" />


## Introduction

This project demonstrates the deployment of a mini honeynet and Security Operations Center (SOC) environment in Microsoft Azure to simulate and analyze real-world cyberattacks. Virtual machines were configured to intentionally generate failed login attempts, and security logs were collected in a centralized Log Analytics Workspace (LAW). Using Microsoft Sentinel, the logs were queried with Kusto Query Language (KQL) to detect suspicious activity, enrich data with geolocation through a GeoIP watchlist, and visualize global attack patterns on an interactive attack map. This project highlights practical skills in log analysis, SIEM configuration, and threat visualization within a cloud-based environment.

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machine
- Log Analytics Workspace
- Microsoft Sentinel

**Query used to detect suspicious actvity:**
```kql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent
    | where IpAddress == <attacker IP address>
    | where EventID == 4625
    | order by TimeGenerated desc
    | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
WindowsEvents
```

  
<img width="2242" height="1279" alt="Screenshot 2025-10-22 at 1 41 11 PM" src="https://github.com/user-attachments/assets/5ccdfe9a-8489-4228-ac49-c7c900819f19" />


# Microsoft Sentinel Global Map Displaying Attacks on the Virtual Machine


<img width="931" height="713" alt="Screenshot 2025-10-22 at 10 51 28 AM" src="https://github.com/user-attachments/assets/b1e4c821-8c36-47e0-b03c-ae6e86a16d49" />
  

## Conclusion

In this project, I deployed a mini honeynet in Microsoft Azure and integrated multiple log sources into a centralized Log Analytics Workspace. Microsoft Sentinel was used to monitor activity, trigger alerts, and automatically create incidents based on ingested logs. Security controls were then implemented to harden the environment, followed by a second round of monitoring to measure the impact. The results showed a significant reduction in security events and incidents, demonstrating the effectiveness of the applied controls in reducing attack surface.

It’s important to note that if the network resources had been actively used by regular users, the volume of security events and alerts during the post-implementation period would likely have been higher.
