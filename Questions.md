tags to elaborate the notes
- [ ] kinds of load balancers.
- [ ] iam. identity options 
- [x] ec2.storage. hardware types
- [ ] databases
- [ ] connections, vpn, hardware, etc
- [ ] key management 
1. ![[Pasted image 20260629111851.png]]
IAM Identity Center: This service simplifies user management by centralizing credentials and access control.

Permission Sets: You can create granular permission sets that align with the principle of least privilege, ensuring that each team has only the access they need.

Group Assignments: By assigning teams to groups with specific permission sets, you streamline access management and reduce the complexity of individual user permissions.

This approach minimizes operational overhead while maintaining secure and compliant access to sensitive customer data

2. ![[Pasted image 20260629113506.png]]
Selected Answer: B

A - For transactional or low-latency queries like product catalog searches in an ecommerce website, Redshift is not suitable because it's a data warehouse solution optimized for analytical queries.

B - ElastiCache for Redis is a highly performant, in-memory caching service that can significantly reduce database load by caching frequent queries.

C - Adding more web servers can't help alleviate the load on the database. The database remains the bottleneck for product catalog queries.

D - If you turn on throttle queries, customers may gradually ask for a refund :) Let's get rid of the bad things in database instead of user experience.

BTW, what Lazy Loading means: Data is added to the cache only when requested. If not found in the cache, it is fetched from the database and added to the cache for subsequent requests.

3. ![[Pasted image 20260629113905.png]]
Selected Answer: D

3 reasons why I will choose Option D:

- 1 - Highly Available and Scalable: EFS is a managed file system designed for high availability and can automatically scale to accommodate the needs of multiple EC2 instances in an Auto Scaling group, which perfectly aligns with the company's requirement.

- 2 - No code changes: Since EFS is a network file system, the application doesn't need significant code changes to access the data stored on it, as it can mount the file system like a local disk.

- 3 - General Purpose performance mode: This mode provides a balance between cost and performance, suitable for most application workloads
4. ![[Pasted image 20260629114226.png]]
Selected Answer: C

A - Spot Instance is never a good idea for databases. These instances can be terminated with little notice, while databases are almost always persistent workloads.

B - On-Demand Capacity is suitable for steady or predictable workloads, but not as cost-efficient as Aurora Serverless for spiky or unpredictable workloads.

C - Aurora Serverless is a fully managed relational database that scales automatically with traffic. Automated Backups is also integrated with Aurora. Good!

D - We need a structured database, not NoSQL database.

5. ![[Pasted image 20260629114515.png]]
AM Roles Anywhere allows on-premises servers and applications to obtain temporary AWS credentials and access AWS resources securely. This solution allows your on-premises virtual machines to use IAM roles without needing long-term credentials (like access keys). The virtual machines can assume roles and access the S3 bucket temporarily and securely.

Since the company is already using AWS IAM Identity Center, using IAM Roles Anywhere allows the company to leverage its existing Identity Center setup while following AWS best practices for security. This approach ensures the application can securely retrieve credentials without embedding static credentials into the application.

6. ![[Pasted image 20260629115156.png]]
[docs](https://aws.amazon.com/blogs/mt/implement-aws-resource-tagging-strategy-using-aws-tag-policies-and-service-control-policies-scps/)
7. ![[Pasted image 20260629115847.png]]
Data transfer within the zone is free, but cross-zone is not free: Same Region ≠ Free Data Transfer

https://aws.amazon.com/blogs/architecture/overview-of-data-transfer-costs-for-common-architectures/

8. ![[Pasted image 20260629122540.png]]
9. ![[Pasted image 20260629122846.png]]
Selected Answer: D

New AWS accounts need consistent, quick, and cost-effective access to these services, so we can't do something for each of these accounts. We should find a way to manage these accounts fast.

A - Managing separate DX connections for every account is too much work and DX connections are expensive.

B - VPC endpoints are for accessing AWS services, not on-premises services.

C - Managing individual VPNs for multiple accounts is too much work and VPNs bring more latency.

D - Transit Gateway simplifies the network by acting as a hub for all VPCs and the on-premises network. Instead of multiple DX or VPN connections, you're using one centralized DX connection with Transit Gateway. The best part is that you can easily add new accounts or VPCs without reconfiguring DX or VPN connectio


 ![[Pasted image 20260629123207.png]]

![[Pasted image 20260629123311.png]]
https://aws.amazon.com/fsx/lustre/