**Note:** For the screenshots, you can store all of your answer images in the `answer-img` directory.

## Verify the monitoring installation


To verify the monitoring installation, Kubernetes resources were inspected to ensure that all monitoring components were running successfully.

The following command was executed:

```bash
kubectl get pods,svc -n monitoring


## Setup the Jaeger and Prometheus source
Grafana was exposed locally using kubectl port-forward on port 3000:

kubectl port-forward svc/grafana 3000:80 -n monitoring

After logging into Grafana, Prometheus was configured as a data source using the Prometheus service running in the monitoring namespace. The data source configuration was validated successfully, confirming that Grafana can query Prometheus metrics.

Grafana was exposed using kubectl port-forward on port 3000. After logging into Grafana, Prometheus was configured as a data source using the Prometheus service running in the monitoring namespace. The data source configuration was validated successfully.

## Create a Basic Dashboard
A basic Grafana dashboard was created using Prometheus as the data source.
This dashboard was used to verify that metrics are being collected and displayed correctly.
The dashboard includes a simple panel querying Prometheus metrics to confirm end‑to‑end connectivity.

## Describe SLO/SLI
A Service Level Objective (SLO) defines the target reliability or performance that a service is expected to achieve. A Service Level Indicator (SLI) is the actual metric used to measure whether the system meets that objective.
For a monthly uptime SLO, the SLI is the percentage of time the application is available and responding correctly during the month.
For request response time, the SLI is the measured latency of requests (for example p95 or p99 latency), which reflects the user experience when interacting with the application.

## Creating SLI metrics.
To measure the defined SLIs, the following five metrics were selected:


Service availability (%)
Measures the percentage of time the service is reachable and operational.


Request latency (p95 / p99)
Captures high‑percentile response times to reflect worst‑case user experience.


HTTP 4xx error rate
Indicates client‑side issues such as invalid requests.


HTTP 5xx error rate
Represents server‑side failures and impacts overall reliability.


Request throughput (requests per second)
Helps correlate traffic patterns with latency and error behavior.

## Create a Dashboard to measure our SLIs
A Grafana dashboard was created to measure SLI metrics over a 24‑hour period.
The dashboard includes panels for:

Frontend service availability
Backend service availability
HTTP 4xx error rate
HTTP 5xx error rate
Request latency

This dashboard provides visibility into system reliability and performance trends.

## Tracing our Flask App

Tracing was implemented using Jaeger to observe backend request processing.

Jaeger was successfully deployed and verified at the backend level. The Jaeger query endpoint responds correctly on port `16686`, as validated using the following command:

```bash
curl http://localhost:16686
``
Due to limitations of the lab environment, the Jaeger UI frontend does not render in the browser, even though the backend and query services are fully operational and responding correctly with HTTP 200.
Despite this frontend limitation, the deployment confirms that Jaeger is running correctly and able to receive and process trace data.
A Python example was used to create a trace and span on the backend to measure request execution time and demonstrate tracing instrumentation.

## Jaeger in Dashboards
Although the Jaeger UI frontend does not render in the lab environment, tracing data can still be correlated with performance metrics.
The Grafana dashboard includes request latency panels, allowing correlation between observed latency and backend processing behavior that would normally be analyzed through Jaeger traces.
This approach still provides insight into performance bottlenecks and service behavior.

## Report Error
TROUBLE TICKET
Name: Jorge Martínez
Date: 15 April 2026
Subject: Backend latency and error rate increase
Affected Area: Backend Flask Service
Severity: Medium
Description:
During normal operation, increased HTTP 4xx and 5xx error rates were observed, along with higher request latency.
Metrics indicate that backend request processing time is the main contributor to the latency.
Tracing and performance metrics allow identification of problematic requests and backend behavior contributing to failures.


## Creating SLIs and SLOs
To guarantee a 99.95% monthly uptime SLO, the following SLIs were selected:

Service availability percentage
Request success rate (non‑error responses)
Backend request latency (p95/p99)
Server error rate (HTTP 5xx

## Building KPIs for our plan
The following KPIs were defined to measure reliability and performance:


Monthly Availability KPI
Tracks uptime directly against the 99.95% SLO.


p95 Request Latency KPI
Measures performance impact on end users.


Error Budget Burn Rate KPI
Tracks how quickly failures consume the allowed error budget.


These KPIs were selected because they provide actionable insight into service health.

## Final Dashboard

A final Grafana dashboard was created to visualize KPIs, SLIs, and SLOs.
The dashboard includes:

Service uptime metrics
Latency percentiles
HTTP 4xx and 5xx error rates
Request throughput

This dashboard provides a consolidated view of service reliability and performance  
