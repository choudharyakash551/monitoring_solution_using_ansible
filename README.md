# Ansible Playbook: Deploy Prometheus and Grafana

This Ansible playbook automates the deployment of Prometheus and Grafana on a local machine. It performs the following steps:

1. **Install Prerequisites:** Installs the necessary packages (`wget` and `gnupg2`) required for the deployment.

2. **Download Grafana tar.gz file:** Downloads the Grafana tar.gz file from the specified URL.

3. **Extract Grafana tar.gz file:** Unarchives the downloaded Grafana tar.gz file to the `/opt/` directory and renames the extracted directory to `/opt/grafana`.

4. **Rename Grafana Directory:** Renames the extracted Grafana directory to `/opt/grafana`.

5. **Start and enable Grafana service:** Starts and enables the Grafana service to ensure it's running and will start automatically on system boot.

6. **Download Prometheus tar.gz file:** Downloads the Prometheus tar.gz file from the specified URL.

7. **Extract Prometheus tar.gz file:** Unarchives the downloaded Prometheus tar.gz file to the `/opt` directory and renames the extracted directory to `/opt/prometheus`.

8. **Rename Prometheus Directory:** Renames the extracted Prometheus directory to `/opt/prometheus`.

9. **Start and enable Prometheus service:** Starts and enables the Prometheus service to ensure it's running and will start automatically on system boot.

## Usage

1. Ensure Ansible is installed on your local machine.
2. Update the `prometheus_version` and `grafana_version` variables in the playbook according to the desired versions.
3. Run the playbook using the command `ansible-playbook playbook.yml`.

## Requirements

- Ansible
- Internet connection (for downloading Grafana and Prometheus tar.gz files)

# Prometheus Setup

## Alerting Rules

### HighCPUUsage
- **Expression:** Calculates the CPU usage percentage and alerts if it exceeds 90% for more than 5 minutes.
- **Severity:** Warning
- **Description:** Indicates high CPU usage detected on the system.

### HighMemoryUsage
- **Expression:** Calculates the memory usage percentage and alerts if it exceeds 90% for more than 5 minutes.
- **Severity:** Warning
- **Description:** Indicates high memory usage detected on the system.

### LowDiskSpace
- **Expression:** Calculates the disk space usage percentage and alerts if it falls below 10% for more than 15 minutes.
- **Severity:** Critical
- **Description:** Indicates low disk space available on the system.

### HighNetworkTraffic
- **Expression:** Calculates the incoming network traffic rate and alerts if it exceeds 100 Mbps for more than 10 minutes.
- **Severity:** Warning
- **Description:** Indicates high network traffic detected on the system.

### ServiceUnavailable
- **Expression:** Alerts if the service is unavailable (i.e., if the up metric is 0).
- **Severity:** Critical
- **Description:** Indicates service unavailability detected on the system.

## Prometheus Configuration

The `prometheus.yml` file configures Prometheus to scrape metrics from its own endpoint (`localhost:9090`) and from a Python Flask application running on port `5000`. Additionally, it specifies the evaluation interval for alerting rules. Commented sections for loading external rule files, configuring Alertmanager, and defining global settings such as scrape intervals are included.

Make sure to adjust the `scrape_configs` section in `prometheus.yml` to scrape metrics from your actual targets.

# Grafana Setup

Grafana is a powerful open-source analytics and monitoring platform. After deployment, Grafana is accessible via a web browser, providing a user-friendly interface to visualize and analyze Prometheus metrics.

## Accessing Grafana Dashboard

Once the playbook execution is complete, you can access Grafana by navigating to `http://<your-server-ip>:3000` in your web browser. The default credentials are usually `admin` for both username and password.

## Importing Real-time Monitoring Dashboard

To import the provided real-time monitoring dashboard:

1. Log in to Grafana using your credentials.
2. Navigate to the "Dashboard" section.
3. Click on "Manage" and then "Import".
4. Copy and paste the provided JSON configuration into the text area.
5. Review the settings and click "Import" to create the dashboard.

# Real-time Monitoring Dashboard

## Overview

This dashboard provides real-time monitoring and visualization of various metrics collected by Prometheus. It includes panels for displaying CPU usage trends, alert status, application status, service availability, and job metrics.

## Panels

1. **Flask App Instance Matrix (Gauge Panel):**
   - Description: Displays the current status of Flask application instances as a gauge.
   - Metric: `ALERTS{instance="localhost:5000"}`.
   - Thresholds: Green (Normal), Red (Alert triggered if metric value exceeds 80).

2. **Alerts in Pending State (Time Series Panel):**
   - Description: Shows alerts currently in a pending state.
   - Metric: `ALERTS{alertstate="pending"}`.
   - Time Range: Last 24 hours.

3. **Flask App Application Status (Table Panel):**
   - Description: Displays the status of the Flask application.
   - Metric: `ALERTS{instance="localhost:5000"}`.
   - Sorting: Descending order based on the severity of alerts.

4. **Alerts in Firing State (Time Series Panel):**
   - Description: Visualizes alerts currently in a firing state.
   - Metric: `ALERTS{alertstate="firing"}`.
   - Time Range: Last 24 hours.

5. **Unavailable Services (Time Series Panel):**
   - Description: Shows the status of unavailable services.
   - Metric: `ALERTS{alertname="ServiceUnavailable"}`.
   - Time Range: Last 24 hours.

6. **Flask App Job Matrix (Histogram Panel):**
   - Description: Displays the status of Flask application jobs in a matrix format.
   - Metric: `ALERTS{job="flask_app"}`.
   - Time Range: Last 24 hours.

## Usage

Use the Grafana dashboard to monitor and analyze the real-time performance of your system and applications. Explore the various panels to gain insights and quickly identify any issues or anomalies that require attention.

## Author

This Ansible playbook was created by Akash Choudhary.
