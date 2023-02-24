# What is PromQL ?

- PromQL is a query language for Prometheus monitoring system. It is designed for building powerful yet simple queries for graphs, alert or derived time series.

## Data Types

PromQLuses for data types

- **Scalar** : The expressions resulting in a single constant numeric floating number is scalar. **e.g, 11.99**
- **Range vector** : These vector have a list of values corresponding to each timestamp in the time series.
- **Instant vector** : These vector have a single value corresponding to each timestamp in the time series.
- **String** The expressions whose output is a string literal is a part of this category. It is current unused in Prometheus. **eg, welcome**

**Example**

```shell
 prometheus_http_requests_total

 prometheus_http_requests_total[1m]
```

# Prometheus Selectors

- Selectors help users to narrow down the results of an expression. For example **prometheus_http_requests_total{handler="/alerts"}** is a selector. Which will result in an all time-series having the metric name **prometheus_http_request_total** and label handler with value/alerts.

```shell
process_cpu_seconds_total
process_cpu_seconds_total{job="Node Exporter"}
```

## Types of Matchers

- **Equality matcher(=)**

```shell
process_cpu_seconds_total{job!="Node Exporter"}
```

- **Negative Equality matcher(!=)**

```shell
prometheus_http_requests_total{handler=~"/api.*"}
```

- **Regular expression matcher(=~)**

```shell
prometheus_http_requests_total{handler=~"/api.*"}
```

- **Negative Regular expression matcher(!~)**

```shell
prometheus_http_requests_total{handler!~"/api.*"}
```

# Binary Operators

- Binary Operators are the operators that take two operands and performs the specified calculations on them.
- Prometheus is query language supports basic logical and arithmetic operators. For operations between two instant vectors, the matching behavior can be modified.

## Binary Operators

There are three types of Binary Operators

- **Arithmetic binary operators**
- **Comparison binary operators**
- **Logical/set binary operators**

### Arithmetic binary operators

Arithmetic operators are the symbols that represent arithmetic math operations.

- - (addition)
- - (subtraction)
- - (multiplication)
- / (division)
- % (modulo)
- ^ (power/exponentiation)

```shell
prometheus_http_requests_total*2
```

### Comparison binary operators

Comparison operators is a mathematical symbols which is used for comparison.

- == (equal)
- != (not-equal)
- > (greater-than)
- < (less-than)
- > = (greater-or-equal)
- <= (less-or-equal)

```shell
prometheus_http_requests_total > 12
node_cpu_seconds_total == 2.19
```

### Logical/set binary operators

The logicial operators are used to combine simple relational expression into more complex expression.

- and (intersection)
- or (union)
- unless(complement)

```shell
vector 1 and vector 2, vector 1 or vector 2, vector 1 unless vector 2

prometheus_http_requests_total or node_cpu_seconds_total
```

# Aggregation Operators

Aggregation Operators calculate mathematical values over a time range.

- sum (calculate sum over dimensions)
- min (select minimum over dimensions)
- max (select maximum over dimensions)
- avg (calculate the average over dimensions)
- group (all values in the resulting vector are 1)
- stddev (calculate population standard deviation over dimensions)
- stdvar (calculate population standard variance over dimensions)
- count (count number of elements in the vector)
- count_values(count number of elements with the same value)
- bottomk (smallest k elements by sample value)
- topk (largest k elements by sample value)

```shell
sum(prometheus_http_requests_total) by (code)
topk(3, sum(prometheus_http_requests_total) by (code))
max(node_cpu_seconds_total)
```

# Functions Changes - deriv(), predict_linear

```shell
changes(node_cpu_seconds_total{job="Node_Exporter"}[1m])

deriv(process_resident_memory_bytes{job="prometheus"}[1h])

predict_linear(node_memory_MemFree_bytes{job="Node_Exporter"}[1h],2*60*60)/1024/1024
```



