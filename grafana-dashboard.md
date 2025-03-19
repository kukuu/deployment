# Grafana Dashboard

Creating dashboards in Grafana to monitor application performance involves connecting Grafana to a data source (like Prometheus), designing panels to visualize key metrics, and organizing them into a cohesive dashboard. Here is a step-by-step guide to creating a dashboard based on the metrics exposed by Prometheus in the previous example

**- Step 1: Set Up Grafana**

1. Deploy Grafana in Kubernetes:

If not already deployed, install Grafana using Helm:

```
helm install grafana grafana/grafana
```

2. Access Grafana:

i. Port-forward the Grafana service:

```
kubectl port-forward svc/grafana 3000:80
```

ii. Open http://localhost:3000 in your browser.

iii. Log in with the default credentials (admin/admin), and change the password when prompted.


**- Step 2: Add Prometheus as a Data Source**

1. Navigate to Data Sources:

i. In Grafana, go to Configuration > Data Sources.

ii. Click Add data source.

iii. Select Prometheus:

a. Choose Prometheus from the list of data sources.

b. Configure Prometheus:

c. Set the URL to the Prometheus service (e.g., http://prometheus:9090).

d. Click Save & Test to verify the connection.

**- Step 3: Create a Dashboard**

1. Create a New Dashboard:

a. Go to Dashboards > New Dashboard.

b. Click Add new panel.

2. Add Panels for Key Metrics:

a. For each metric, create a panel to visualize it. Below are examples of common metrics and their corresponding Prometheus queries:

**_Panel 1: Request Rate_**

i. Query:

```
rate(http_requests_total[1m])
```
ii. Visualization:

a. Choose Time series or Graph.

b. Set the title to "Request Rate".

3. Customize the display (e.g., line thickness, colors).

**_Panel 2: Error Rate_**

Query:

```
rate(http_requests_total{status=~"5.."}[1m])
```

**_Visualization:_**

i. Choose Time series or Graph.

ii. Set the title to "Error Rate".

iii. Use a red color to highlight errors.

**_Panel 3: CPU Usage_**

Query:

```
rate(process_cpu_seconds_total[1m])
```

**_Visualization:_**

i. Choose Gauge or Stat.

ii. Set the title to "CPU Usage".

iii. Set thresholds for warning (e.g., 70%) and critical (e.g., 90%).

**_Panel 4: Memory Usage_**

Query:

```
process_resident_memory_bytes

```

**_Visualization:_**

i. Choose Gauge or Stat.

ii. Set the title to "Memory Usage".

iii. Convert bytes to megabytes or gigabytes for readability.

**_Panel 5: Response Time_**

Query:

```
histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[1m]))

```

**_Visualization:_**

i. Choose Time series or Graph.

ii. Set the title to "99th Percentile Response Time".

iii. Use milliseconds or seconds for the Y-axis.

3. Organize Panels:

i. Drag and drop panels to arrange them logically.

ii. Group related metrics (e.g., request rate and error rate) together.


4. Add Annotations (Optional):

i. Use annotations to mark significant events (e.g., deployments, incidents).

ii. Example: Add an annotation for when a new version was deployed.


**_Step 4: Save and Share the Dashboard_**

i. Save the Dashboard:

a. Click Save at the top of the page.

b. Give the dashboard a name (e.g., "Application Performance Dashboard").

3. Share the Dashboard:

i. Click the Share button to generate a link or export the dashboard as JSON.

ii. You can also set up public access or embed the dashboard in other tools.

**_Step 5: Set Up Alerts (Optional)_**

i. Create Alert Rules:

a. Go to Alerting > Alert rules.

ii. Define conditions for alerts (e.g., error rate > 5%).

iii. Configure Notification Channels:

a. Set up channels for notifications (e.g., email, Slack, PagerDuty).

b. Example: Send an alert to Slack if the error rate exceeds a threshold.
