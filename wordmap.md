-Multi-AZ → _Amazon RDS, EC2, ElastiCache_ — provides automatic failover within a Region; never confound with multi-Region DR.

-Multi-Region → _S3 Cross-Region Replication, Aurora Global Database, Route 53 failover_ — used for global disaster recovery and data sovereignty.

-Auto Scaling Group (ASG)/Fargate → _EC2 Auto Scaling/ECS_ — ensures elasticity and resilience by maintaining desired capacity automatically.

-Load Balancer → _Elastic Load Balancing (ALB / NLB)_ — removes single points of failure by distributing traffic across targets.

-Route 53 → _Amazon Route 53_ — DNS-level health checks, latency-based, and weighted routing for resilient global access.

-Least Privilege → _AWS IAM, SCPs_ — always grant only the permissions required; never reuse broad roles.

-KMS (Encryption) → _AWS Key Management Service_ — encrypt data at rest (S3 SSE-KMS, EBS encryption, RDS KMS)

-IAM Role vs User → _IAM Roles + STS_ — temporary credentials for apps and cross-account access; no shared access keys.

-Backup Vault Lock → _AWS Backup_ — makes backups immutable for compliance (prevents deletion/modification).

-CloudTrail + S3 Object Lock → _AWS CloudTrail_ audit logs stored in _S3_ with Object Lock for tamper-proof evidence.

-Intelligent-Tiering → _Amazon S3 Intelligent-Tiering_ — automatic tiering of objects based on access patterns.

-Lifecycle Policy → _S3 Lifecycle Rules_ — transitions data to Glacier / Deep Archive for cost optimization.

-Cross-Region Replication (CRR) → _S3 CRR_ — keeps a replica bucket in another region for DR purposes.

-Read Replica → _Amazon RDS, Aurora_ — offload read traffic or promote to standby for DR.

-AWS Backup → _AWS Backup Service_ — centralized backup orchestration across EBS, RDS, DynamoDB, EFS.

-Global Accelerator → _AWS Global Accelerator_ — routes user traffic through AWS’s global network to reduce latency for a single-region app.

-CloudFront → _Amazon CloudFront_ — content delivery network caching static and dynamic content near users.

-VPC Endpoint → _AWS PrivateLink / VPC Endpoints_ — enables private connectivity to AWS services without using the public Internet.

-Cost Explorer / Trusted Advisor → _AWS Cost Explorer, AWS Trusted Advisor_ — analyze and optimize costs, detect under-utilized resources.

-Well-Architected Framework (WAF) → _AWS Well-Architected Tool_ — assess workloads across six pillars:  
Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability