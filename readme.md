# How does Prometheus work?

```bash
pyenv virtualenv 3.8.11 prometheus
pyenv local prometheus
pip install prometheus-client
python
```

```python
from prometheus_client import start_http_server, Summary

summary = Summary("this_summary_name", "This summary's docs")

start_http_server(8001)
# view http://localhost:8001/
## this_summary_name_count 0.0
## this_summary_name_sum 0.0

summary.observe(7)
## this_summary_name_count 1.0
## this_summary_name_sum 7.0

summary.observe(11)
## this_summary_name_count 2.0
## this_summary_name_sum 18.0

summary.observe(13)
## this_summary_name_count 3.0
## this_summary_name_sum 31.0
summary.observe(-9)
## this_summary_name_count 4.0
## this_summary_name_sum 22.0


# hmm; can I add a counter live?
from prometheus_client import Counter
counter = Counter("this_counter_name", "This counter's docs")
# yes!

## this_counter_name_total 0.0
counter.inc()
## this_counter_name_total 1.0
counter.inc(7)
## this_counter_name_total 8.0
counter.inc(-9)
## ValueError: Counters can only be incremented by non-negative amounts.


from prometheus_client import Gauge
gauge = Gauge("this_gauge_name", "This gauge's docs")
## this_gauge_name 0.0
# inc, dec, set...


from prometheus_client import Histogram
histogram = Histogram("this_histogram_name", "This histogram's docs")
## this_histogram_name_bucket{le="0.005"} 0.0
## this_histogram_name_bucket{le="0.01"} 0.0
## this_histogram_name_bucket{le="0.025"} 0.0
## this_histogram_name_bucket{le="0.05"} 0.0
## this_histogram_name_bucket{le="0.075"} 0.0
## this_histogram_name_bucket{le="0.1"} 0.0
## this_histogram_name_bucket{le="0.25"} 0.0
## this_histogram_name_bucket{le="0.5"} 0.0
## this_histogram_name_bucket{le="0.75"} 0.0
## this_histogram_name_bucket{le="1.0"} 0.0
## this_histogram_name_bucket{le="2.5"} 0.0
## this_histogram_name_bucket{le="5.0"} 0.0
## this_histogram_name_bucket{le="7.5"} 0.0
## this_histogram_name_bucket{le="10.0"} 0.0
## this_histogram_name_bucket{le="+Inf"} 0.0
## this_histogram_name_count 0.0
## this_histogram_name_sum 0.0
histogram.observe(4.7)
## this_histogram_name_bucket{le="0.005"} 0.0
## this_histogram_name_bucket{le="0.01"} 0.0
## this_histogram_name_bucket{le="0.025"} 0.0
## this_histogram_name_bucket{le="0.05"} 0.0
## this_histogram_name_bucket{le="0.075"} 0.0
## this_histogram_name_bucket{le="0.1"} 0.0
## this_histogram_name_bucket{le="0.25"} 0.0
## this_histogram_name_bucket{le="0.5"} 0.0
## this_histogram_name_bucket{le="0.75"} 0.0
## this_histogram_name_bucket{le="1.0"} 0.0
## this_histogram_name_bucket{le="2.5"} 0.0
## this_histogram_name_bucket{le="5.0"} 1.0
## this_histogram_name_bucket{le="7.5"} 1.0
## this_histogram_name_bucket{le="10.0"} 1.0
## this_histogram_name_bucket{le="+Inf"} 1.0
## this_histogram_name_count 1.0
## this_histogram_name_sum 4.7
histogram.observe(0.02)
## this_histogram_name_bucket{le="0.005"} 0.0
## this_histogram_name_bucket{le="0.01"} 0.0
## this_histogram_name_bucket{le="0.025"} 1.0
## this_histogram_name_bucket{le="0.05"} 1.0
## this_histogram_name_bucket{le="0.075"} 1.0
## this_histogram_name_bucket{le="0.1"} 1.0
## this_histogram_name_bucket{le="0.25"} 1.0
## this_histogram_name_bucket{le="0.5"} 1.0
## this_histogram_name_bucket{le="0.75"} 1.0
## this_histogram_name_bucket{le="1.0"} 1.0
## this_histogram_name_bucket{le="2.5"} 1.0
## this_histogram_name_bucket{le="5.0"} 2.0
## this_histogram_name_bucket{le="7.5"} 2.0
## this_histogram_name_bucket{le="10.0"} 2.0
## this_histogram_name_bucket{le="+Inf"} 2.0
## this_histogram_name_count 2.0
## this_histogram_name_sum 4.72
```


https://github.com/prometheus/client_python

> "The Python client doesn't store or expose quantile information at
> this time."

rolls eyes
