# SSH Brute Force Analysis in Splunk

This project shows how I used Splunk Enterprise to investigate SSH brute force activity on a Linux system. I uploaded Linux authentication logs, wrote SPL searches to extract important details, and built a dashboard that shows the attack patterns.

## Overview

The goal of this project was to understand how brute force attacks appear in Linux logs and how to analyze them using Splunk. I focused on:

- finding which IPs generated the most failed logins  
- identifying the usernames attackers tried  
- checking invalid or unknown user attempts  
- tracking activity over time  
- confirming if any logins were successful  
- documenting everything in an incident style format  

The logs showed automated attacks on the root account and several invalid usernames. No successful logins were found.

## Dashboard Panels

The dashboard includes:

- top attacking IP addresses  
- attempted usernames  
- unknown or invalid user attempts  
- SSH authentication failures over time  
- successful login checks  
- total failure counts  
- invalid username attempts over time  
- most attempted invalid usernames  
- attacking IP activity over time  
- message breakdown from the logs  

## SPL Queries Used

### Top Attacking IPs

sourcetype="linuxsyslog" "authentication failure"
| rex "rhost=(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count as failures by src_ip
| sort -failures

### Attempted Usernames

sourcetype="linuxsyslog" ("authentication failure" OR "user unknown")
| rex "user=(?<user>\S+)"
| stats count by user
| sort -count

### Failures Over Time

sourcetype="linuxsyslog" "authentication failure"
| timechart span=30m count

## Findings

- multiple external IPs attempted to brute force the system  
- most attempts targeted the root account  
- attackers tried invalid usernames like guest and test  
- over 300 attempts were from nonexistent users  
- no successful logins appeared in the logs  
- behavior matched typical automated scanning and password guessing  

## Tools Used

- Splunk Enterprise  
- SPL  
- Linux authentication logs  

## Incident Notes

A full writeup is included in the incident report file. It explains the five W breakdown and recommended actions such as disabling root SSH login, using fail2ban, and restricting SSH access.

## What I Learned

- how to read and analyze Linux logs  
- how to write useful SPL searches  
- how to build SOC style dashboards  
- how to recognize brute force attack patterns  
- how to document incidents clearly  
