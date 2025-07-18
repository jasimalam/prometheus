## PROMETHEUS METRIC TYPES

### 1. Counter

* Monotonically increasing value (resets on restart).
* Example: `http_requests_total`, `node_network_receive_bytes_total`

**Query Example:**

```promql
rate(http_requests_total[1m])
```

Shows per-second request rate over the last 1 minute.

---

### 2. Gauge

* A value that can go up and down.
* Example: `node_memory_Active_bytes`, `container_memory_usage_bytes`

**Query Example:**

```promql
100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)
```

Shows memory usage percentage.

---

### 3. Histogram

* Observes value distribution over defined buckets.
* Example: `http_request_duration_seconds_bucket`

**Query Example:**

```promql
rate(http_request_duration_seconds_bucket[5m])
```

Shows request rate grouped by duration.

---

### 4. Summary

* Similar to histogram, stores quantiles directly.
* Less flexible for aggregations in PromQL.

---

## PROMQL FUNCTIONS

### rate()

Used with counters to calculate per-second change.

```promql
rate(http_requests_total[5m])
```

---

### sum()

Adds up all series values.

```promql
sum(rate(http_requests_total[1m]))
```

---

### avg()

Computes the average across series.

```promql
avg(rate(node_cpu_seconds_total{mode="user"}[5m]))
```

---

### max()

Returns the highest value among series.

```promql
max(container_memory_usage_bytes)
```

---

### min()

Returns the lowest value.

```promql
min(node_memory_MemAvailable_bytes)
```

---

### count()

Counts the number of time series.

```promql
count(container_cpu_usage_seconds_total)
```

---

### increase()

Total increase over a range (counters).

```promql
increase(http_requests_total[1h])
```

---

### topk()

Top N series by value.

```promql
topk(5, rate(container_cpu_usage_seconds_total[5m]))
```

---

### bottomk()

Bottom N series by value.

```promql
bottomk(5, rate(http_requests_total[1m]))
```

---

### deriv()

Estimates the slope (change rate), best for noisy data.

```promql
deriv(http_requests_total[5m])
```

---

## PRACTICAL EXAMPLES

### CPU Usage (Node level)

```promql
rate(node_cpu_seconds_total{mode!="idle"}[5m])
```

### Memory Usage (%)

```promql
100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)
```

### Disk Usage (%)

```promql
100 - (node_filesystem_free_bytes{mountpoint="/"} * 100 / node_filesystem_size_bytes{mountpoint="/"})
```

### Network In/Out Rate

```promql
rate(node_network_receive_bytes_total[1m])
rate(node_network_transmit_bytes_total[1m])
```

### Pod Restart Count

```promql
kube_pod_container_status_restarts_total
```

### Top 5 Pods by CPU Usage

```promql
topk(5, rate(container_cpu_usage_seconds_total{image!=""}[5m]))
```

### Number of Running Pods

```promql
count(kube_pod_status_phase{phase="Running"})
```

### Slow HTTP Requests (>1s)

```promql
sum(rate(http_request_duration_seconds_bucket{le="1"}[5m]))
```

### Percentage of Slow Requests (>1s)

```promql
100 * (
  sum(rate(http_request_duration_seconds_bucket{le="1"}[5m])) 
/
  sum(rate(http_request_duration_seconds_count[5m]))
)
```