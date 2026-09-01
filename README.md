# BUILDING A SIEM DASHBOARD USING ELK STACK

## About the Project

This project is about building a basic SIEM dashboard using the ELK Stack. The main purpose of the project is to collect authentication logs from a Kali Linux system and monitor failed SSH login attempts.

Elastic Agent is used for collecting the logs. The logs are sent to Elasticsearch, where they are stored. Kibana is then used to view the logs, create visualizations and build the final dashboard.

A detection rule was also created to identify repeated failed SSH authentication attempts.

## Technologies Used

* Kali Linux
* Elasticsearch 8.14.3
* Kibana 8.14.3
* Elastic Agent 8.14.3
* Docker
* Docker Compose
* SSH / PAM Authentication
* Kibana Query Language (KQL)

## How the Project Works

The basic flow of the project is:

```text
Kali Linux
    ↓
Elastic Agent
    ↓
Elasticsearch
    ↓
Kibana
    ↓
Dashboard and Security Alerts
```

Authentication and SSH events are generated on the Kali Linux system. Elastic Agent collects these events and sends them to Elasticsearch. The events can then be checked in Kibana Discover.

After analyzing the logs, different visualizations were created and combined into a single SIEM dashboard.

## Authentication Logs

The authentication logs were collected using the `system.auth` dataset.

Some of the fields observed during testing were:

```text
event.action       = ssh_login
event.category     = authentication
event.outcome      = failure
event.dataset      = system.auth
user.name          = wronguser
source.ip          = ::1
host.name          = kali
```

The SSH login failures used for testing were generated locally on the Kali Linux system.

## SSH Brute Force Detection

A detection rule named **SSH Brute Force Detection** was created in Kibana.

The query used was:

```text
event.dataset:"system.auth" AND event.category:"authentication" AND event.outcome:"failure"
```

The rule was configured to generate an alert when more than 5 matching events occurred within 5 minutes.

For testing, six failed SSH authentication events were generated within the configured time period. The alert became active after the threshold was reached.

After the failed login activity was stopped, the alert changed to the recovered state.

## Dashboard

The final dashboard contains 11 panels:

1. Total SSH Authentication Failures
2. Authentication Failures Over Time
3. Top User by Authentication Failure
4. Authentication Failures by Source IP
5. Authentication Failures by Host/Agent
6. Authentication Failures by Event Type
7. Authentication Failures by Process
8. Authentication Outcomes - Success vs Failure
9. Active Security Alerts
10. Active Alerts by Severity
11. Recent SSH Authentication Events

These panels are used to get an overall view of authentication activity and failed SSH login attempts.

## Project Structure

```text
siem-project/
│
├── documentation/
│   └── BUILDING-A-SIEM-DASHBOARD-USING-ELK-STACK-FINAL.pdf
│
├── docker-compose.yml
├── .gitignore
├── config/
├── logs/
└── screenshots/
```

## Documentation

The complete project report is available in the `documentation` folder.

```text
documentation/
└── BUILDING-A-SIEM-DASHBOARD-USING-ELK-STACK-FINAL.pdf
```

The report contains the project introduction, tools and technologies, system architecture, implementation steps, log analysis, detection rule, alerts, dashboard, results and conclusion.

## Security

Sensitive files such as `.env` and files containing passwords, tokens or private keys are not included in this repository.

The `.gitignore` file is used to prevent these files from being added to Git.

## Project Status

The basic SIEM dashboard has been implemented and tested using SSH authentication failure events on Kali Linux.
