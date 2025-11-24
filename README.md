SSH Brute Force Analysis in Splunk

This project shows how I used Splunk Enterprise to investigate SSH brute force activity on a Linux system. I took system authentication logs, uploaded them into Splunk, wrote SPL searches to pull out important details, and built a dashboard that highlights the attack patterns.

Overview

The goal of this project was to understand how brute force attempts show up in Linux logs and how to investigate them using Splunk. I focused on:

finding the IPs that generated the most failed logins

identifying which usernames attackers tried

tracking activity over time

checking for successful logins

documenting everything in an incident style format

The data clearly showed automated attacks against root and several other usernames. No successful logins were found.

Dashboard Panels

The dashboard includes:

Top attacking IP addresses

Attempted usernames

Unknown or invalid user attempts

SSH authentication failures over time

Successful login checks

Total failure counts

Invalid username attempts over time

Most attempted invalid usernames

Activity by IP over time

Message breakdown from the logs

These panels give a full picture of what was happening on the system.

SPL Queries Used
Top Attacking IPs
sourcetype="linuxsyslog" "authentication failure"
| rex "rhost=(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count as failures by src_ip
| sort -failures

Attempted Usernames
sourcetype="linuxsyslog" ("authentication failure" OR "user unknown")
| rex "user=(?<user>\S+)"
| stats count by user
| sort -count

Failures Over Time
sourcetype="linuxsyslog" "authentication failure"
| timechart span=30m count

Findings

Several external IPs attempted to brute force the system

Most attempts targeted the root account

Attackers also tried invalid usernames like guest and test

Over 300 attempts came from nonexistent users

No successful logins appeared in the logs

The pattern matched common automated scanning and password guessing

Tools Used

Splunk Enterprise

SPL searches

Linux system authentication logs

Incident Notes

A full writeup is included in the incident report file. It breaks down what happened using the five W structure and lists recommendations like disabling root SSH login, enabling fail2ban, and limiting SSH access.

What I Learned

This project helped me get more comfortable with reading Linux logs, working with SPL, building dashboards, and recognizing brute force activity. It also helped me practice writing an incident style summary.
