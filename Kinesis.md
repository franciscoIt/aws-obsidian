---

tags:

- aws/analytics
- aws/saa
- streaming aliases:
- Kinesis
- AWS Kinesis created: 2026-07-21 status: studying exam: AWS Certified Solutions Architect - Associate

---

# Amazon Kinesis

> [!abstract] Summary Kinesis is AWS's platform for **real-time data streaming** — collecting, processing, and analyzing streaming data (clickstreams, IoT telemetry, logs, video) as it arrives instead of waiting for a batch job. It's the streaming counterpart to [[Amazon SQS]] (which is queue-based, not stream-based).

## Related notes

- [[AWS SAA]]
- [[Amazon SQS]]
- [[Amazon MSK (Managed Kafka)]]
- [[AWS Lambda]]
- [[Amazon S3]]
- [[Data Analytics on AWS]]

---

## The Four Kinesis Services

> [!question]- Why does Kinesis have 4 sub-services? Click to expand Each solves a different piece of the streaming pipeline: **ingest** → **transform** → **store** → **analyze/view**. The exam loves asking "which Kinesis service fits this scenario."

|Service|Purpose|Key trait|
|---|---|---|
|**Kinesis Data Streams (KDS)**|Real-time ingestion & custom processing|You manage shards/capacity, consumers read with custom code|
|**Kinesis Data Firehose**|Load streaming data into destinations|**Fully managed**, near real-time (not true real-time), no code needed|
|**Kinesis Data Analytics**|Run SQL / Apache Flink on streaming data|Analyze data _in motion_|
|**Kinesis Video Streams**|Ingest & process video streams|For media/ML on video (e.g., Rekognition)|

---

## 1. Kinesis Data Streams (KDS)

> [!tip] Core exam concept KDS is the "raw" streaming service — think of it as a **highly scalable, durable message log** similar to Apache Kafka.

### Key building blocks

- **Stream** — the pipe of data
- **Shard** — a unit of throughput/capacity within a stream
    - Each shard = **1 MB/sec or 1,000 records/sec IN**
    - Each shard = **2 MB/sec OUT** (classic consumers)
    - You can have many shards → total capacity = sum of shards
- **Record** — data blob up to **1 MB**, made of:
    - Partition key (determines which shard)
    - Sequence number (assigned by Kinesis)
    - Data blob
- **Producers** — apps, SDK, Kinesis Producer Library (KPL), Kinesis Agent
- **Consumers** — apps using Kinesis Client Library (KCL), Lambda, or [[Kinesis Data Analytics]]

### Capacity modes

- [ ] **Provisioned mode** — you choose # of shards, pay per shard-hour, must reshard manually to scale
- [ ] **On-demand mode** — auto-scales, pay per stream/hour + data volume, good for unpredictable workloads

> [!warning] Exam trap **Data retention default is 24 hours**, extendable up to **365 days** (1 year). After the retention window, data is gone unless consumed/stored elsewhere. This is different from SQS, which deletes messages once consumed.

### Consumer types

|Type|Behavior|
|---|---|
|**Classic (shared) fan-out**|Consumers pull; 2 MB/sec total per shard shared across all consumers; can hit throughput limits with many consumers|
|**Enhanced fan-out (EFO)**|Each consumer gets its own **dedicated 2 MB/sec pipe** per shard, pushed via HTTP/2 — lower latency (~70ms), higher cost|

### Ordering & scaling

- Ordering is guaranteed **only within a shard** (same partition key → same shard → order preserved)
- To scale: **reshard** (split shards to increase capacity, merge shards to decrease/save cost)

---

## 2. Kinesis Data Firehose

> [!tip] Core exam concept Firehose = **"load and forget."** No custom consumer code — you just point it at a destination.

- **Fully managed**, near-real-time (buffers data — typically **60 sec or up to 128 MB**, whichever first, before delivery)
- Can **transform data** in-flight using a [[AWS Lambda]] function
- Destinations:
    - [[Amazon S3]]
    - Amazon Redshift (via S3 first)
    - OpenSearch Service
    - **Splunk / generic HTTP endpoints / 3rd-party partners** (Datadog, New Relic, etc.)
- **No data storage** — it's a delivery pipe, not a persistent stream (unlike KDS)
- Auto-scales — no shards to manage

### KDS vs Firehose (classic exam question)

|Feature|Kinesis Data Streams|Kinesis Data Firehose|
|---|---|---|
|Latency|Real-time (~200ms)|Near real-time (buffered, ~60s)|
|Management|You manage shards/capacity|Fully managed|
|Storage|Data retained 24hr–365days|No storage, delivery only|
|Consumers|Custom code (KCL, Lambda)|Fixed destinations (S3, Redshift, OpenSearch, Splunk, HTTP)|
|Use case|Custom real-time processing|Simple "stream → data store" pipelines|

---

## 3. Kinesis Data Analytics

- Run **SQL queries** or **Apache Flink applications** directly against streaming data
- Reads from KDS or Firehose, writes results to S3, Redshift, KDS, Firehose, or Lambda
- Use cases: real-time dashboards, anomaly detection, time-series aggregation

---

## 4. Kinesis Video Streams

- Ingest live video/audio from millions of devices (e.g., security cameras, IoT)
- Durable storage + playback
- Integrates with ML services like Amazon Rekognition for analysis
- One producer per video stream (typically), consumers process frames

---

## Kinesis vs SQS vs MSK — the "which one" question

> [!question]- Click for the decision logic AWS wants you to know
> 
> - Need **strict ordering per key + replay + multiple consumers reading independently** → **KDS**
> - Need simple **decoupling of producers/consumers, one-time processing** → **SQS**
> - Already invested in **Apache Kafka**, need full Kafka API compatibility → **Amazon MSK**
> - Need to **just dump streaming data into S3/Redshift/Splunk with no code** → **Firehose**

---

## Security & Integration

- Encryption at rest via **KMS**, in transit via **HTTPS/TLS**
- **IAM** policies control producer/consumer access
- **VPC endpoints** supported for private access
- Integrates with [[AWS Lambda]] as a consumer (event source mapping) — Lambda polls shards

---

## Quick-fire exam facts

- [ ] Record max size: **1 MB**
- [ ] Shard capacity: **1 MB/s or 1,000 rec/s in**, **2 MB/s out**
- [ ] Default retention: **24 hours**; max: **365 days**
- [ ] Firehose buffer: **up to 60 sec / 128 MB**
- [ ] Firehose has **no replay** — once delivered, it's gone from Firehose
- [ ] KDS supports **replay** within retention window (multiple consumers can re-read)
- [ ] Ordering guaranteed **per shard/partition key**, not across the whole stream

---

## Practice scenario prompts

> [!example] Try these before moving on
> 
> 1. A company needs to ingest clickstream data and load it into S3 with minimal management — no custom code. → **?**
> 2. An app needs multiple independent consumers to each process the full stream in real time with dedicated throughput. → **?**
> 3. A team wants to run live SQL aggregations on IoT sensor data. → **?**

<!-- Answers: 1. Firehose 2. KDS with Enhanced Fan-Out 3. Kinesis Data Analytics -->

---

## Study checklist

- [ ] Understand shard math (calculate capacity given records/sec and record size)
- [ ] Know retention period defaults and max
- [ ] Compare KDS vs Firehose vs SQS vs MSK
- [ ] Understand enhanced fan-out vs classic consumers
- [ ] Know Firehose destinations by heart
- [ ] Review Lambda-as-consumer pattern (event source mapping, batch size, parallelization)

#aws/saa #kinesis #streaming