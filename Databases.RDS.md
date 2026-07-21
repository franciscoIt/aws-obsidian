## Amazon RDS (Relational Database Service) — Notes

### What is RDS?

Amazon RDS is a **managed relational database service** that makes it easy to set up, operate, and scale a relational database in the cloud. AWS handles the undifferentiated heavy lifting — patching, backups, hardware provisioning, and failure detection.

---

### Supported Database Engines

- **MySQL**
- **PostgreSQL**
- **MariaDB**
- **Oracle**
- **Microsoft SQL Server**
- **Amazon Aurora** (MySQL & PostgreSQL compatible)

---

### Key Features

#### 1. Managed Infrastructure

- Automated OS and database engine patching
- Automated backups and snapshots
- Automatic host replacement on hardware failure

#### 2. Multi-AZ Deployments (High Availability)

- RDS synchronously replicates data to a **standby instance in a different Availability Zone**
- Provides **automatic failover** during instance failure, AZ outage, or maintenance
- Standby is **not readable** (it's for failover only, not read scaling)

#### 3. Read Replicas (Scalability)

- Asynchronous replication to one or more **read-only copies**
- Used to **offload read traffic** from the primary instance
- Can be in the same region or **cross-region**
- Can be promoted to a standalone primary instance if needed

#### 4. Storage Options (EBS-backed)

|Type|Best For|
|---|---|
|General Purpose SSD (gp2/gp3)|Most workloads, balanced price/performance|
|Provisioned IOPS SSD (io1/io2)|I/O-intensive workloads (OLTP, large DBs)|
|Magnetic (legacy)|Rarely used, backward compatibility|

#### 5. Backups & Recovery

- **Automated backups** — daily snapshots + transaction logs, enables **point-in-time recovery**
- **Manual snapshots** — user-initiated, retained until explicitly deleted
- Snapshots can be **copied across regions/accounts**

#### 6. Security

- Encryption at rest via **AWS KMS**
- Encryption in transit via **SSL/TLS**
- Runs inside a **VPC** with security groups controlling access
- IAM database authentication supported (for MySQL/PostgreSQL)

#### 7. Monitoring

- **Amazon CloudWatch** — CPU, memory, storage, connections
- **Enhanced Monitoring** — OS-level metrics
- **Performance Insights** — query-level performance tuning

---

### RDS vs Running a Database on EC2

| Aspect                                                     | RDS                        | EC2 (self-managed)                   |
| ---------------------------------------------------------- | -------------------------- | ------------------------------------ |
| Patching                                                   | Automated                  | Manual                               |
| Backups                                                    | Automated                  | Manual                               |
| OS-level access                                            | ❌ No                       | ✅ Yes                                |
| Full engine features (e.g., SQL Server native backup, DQS) | ❌ Limited/unsupported      | ✅ Full support                       |
| Scaling                                                    | Vertical + read replicas   | Manual                               |
| Use case                                                   | Standard managed workloads | Custom configs, unsupported features |