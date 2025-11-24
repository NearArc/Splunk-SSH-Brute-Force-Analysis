# Entry 8 | SSH Brute Force Analysis – Splunk Investigation

## Date
11/24/2025

## Description
Analyzed Linux SSH authentication logs within Splunk Enterprise to investigate signs of brute force attacks against a simulated server. Built a custom dashboard that visualized top attacking IPs, targeted usernames, authentication failures over time, and successful login attempts. Multiple attackers were identified attempting to access the system using the root account and several invalid usernames. No successful logins were observed.

## Tools Used
- Splunk Enterprise  
- SPL  
- Regex field extraction  
- Linux SSH syslog data  

## The 5 W's

### Who caused the incident?
Multiple external IP addresses performed automated brute force SSH attempts targeting the server. These appear to be internet wide scanners or bots rather than a targeted attack.

### What happened?
Attackers repeatedly attempted to log in using:
- the root account  
- invalid usernames such as guest and test  
- completely nonexistent users (354 user unknown attempts)

Brute force activity happened in fast bursts, with some IPs attempting between 20 and 240 rapid failures.

### When did the incident occur?
Across multiple dates in June and July 2025. Activity took place at random times, often within seconds of each other, showing automated behavior.

### Where did the incident happen?
On the simulated Linux host combo, according to the SSH syslog authentication logs uploaded into Splunk.

### Why did the incident happen?
These attacks were likely done by automated bots scanning the internet for exposed SSH services with weak or default credentials. The consistent focus on root and rapid login failures indicates password guessing behavior.

## Additional Notes
- The Splunk dashboard showed no successful SSH logins, confirming there was no compromise.  
- The top attacking IP generated 240 failed attempts, while others ranged from 30 to 70 attempts.  
- User unknown attempts show that attackers probed fake usernames before returning to root.  
- The logs reflect typical background noise seen on publicly accessible Linux systems.  

### Recommended Defensive Actions
- Disable root SSH login  
- Enforce key based authentication  
- Implement fail2ban or SSH rate limiting  
- Allowlist approved IPs for SSH access  
- Continue Splunk based monitoring  
