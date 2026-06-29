Managed NFS that can be mounted in EC2
Works in multi-az 
highly available and expensive.

Use cases: content management, web serving, data sharing, workpress
- Uses NFSv4.1 
- Uses security group to control access to EFS 
- Not compatible with Windows
- Encryption at rest using KMS
POSIX file system, with a standard file api
- Scales automatically


# EFS – Performance & Storage Classes

## EFS Scale

- 1000s of concurrent NFS clients, 10 GB+ /s throughput
- Grow to Petabyte-scale network file system, automatically

### Performance Mode

> Set at EFS creation time

- **General Purpose** (default) – latency-sensitive use cases (web server, CMS, etc…)
- **Max I/O** – higher latency, throughput, highly parallel (big data, media processing)

### Throughput Mode

- **Bursting** – 1 TB = 50MiB/s + burst of up to 100MiB/s
- **Provisioned** – set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage
- **Elastic** – automatically scales throughput up or down based on your workloads
    - Up to 3GiB/s for reads and 1GiB/s for writes
## EFS Scale

- 1000s of concurrent NFS clients, 10 GB+ /s throughput
- Grow to Petabyte-scale network file system, automatically

## Performance Mode

> Set at EFS creation time

- **General Purpose** (default) – latency-sensitive use cases (web server, CMS, etc…)
- **Max I/O** – higher latency, throughput, highly parallel (big data, media processing)

## Throughput Mode

- **Bursting** – 1 TB = 50MiB/s + burst of up to 100MiB/s
- **Provisioned** – set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage
- **Elastic** – automatically scales throughput up or down based on your workloads
    - Up to 3GiB/s for reads and 1GiB/s for writes

## Storage Tiers

> Lifecycle management feature – move file after N days

- **Standard** – for frequently accessed files
- **Infrequent Access (EFS-IA)** – cost to retrieve files, lower price to store
- **Archive** – rarely accessed data (few times each year), 50% cheaper
- Implement **lifecycle policies** to move files between storage tiers

## Availability & Durability

- **Standard** – Multi-AZ, great for prod
- **One Zone** – One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA)
- 