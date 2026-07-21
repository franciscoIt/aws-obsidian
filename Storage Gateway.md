---

tags:

- aws/storage
- aws/saa
- hybrid-cloud aliases:
- Storage Gateway
- AWS Storage Gateway created: 2026-07-21 status: studying exam: AWS Certified Solutions Architect - Associate

---

# AWS Storage Gateway

> [!abstract] Summary Storage Gateway is AWS's **hybrid cloud storage** service — it's a virtual appliance (VM or hardware appliance) you run **on-premises** that bridges local applications to AWS storage ([[Amazon S3]], Glacier, [[Amazon EBS]]) using standard storage protocols (NFS, SMB, iSCSI, VTL). The on-prem app thinks it's talking to local storage; behind the scenes, data flows to AWS.

## Related notes

- [[AWS SAA]]
- [[Amazon S3]]
- [[Amazon EBS]]
- [[Amazon FSx]]
- [[AWS DataSync]]
- [[Hybrid Cloud on AWS]]

---

## Why it exists

> [!question]- Why would anyone need this? Click to expand Companies have legacy on-prem apps, backup software, and file shares that can't just be "moved" to the cloud overnight. Storage Gateway lets them **keep using on-prem storage protocols and workflows** while quietly getting cloud backup, archiving, and low-latency caching — a stepping stone toward full migration.

---

## The Four Gateway Types

> [!tip] Core exam concept Each gateway type maps to a **different local protocol** and a **different AWS storage back end**. The exam tests "which gateway type fits this scenario" heavily.

|Gateway Type|Local Protocol|AWS Back End|Use Case|
|---|---|---|---|
|**S3 File Gateway**|NFS / SMB|S3 (with local cache)|Store files as objects in S3, access on-prem as a file share|
|**FSx File Gateway**|SMB|Amazon FSx for Windows File Server|Low-latency on-prem access to FSx Windows shares|
|**Volume Gateway – Cached**|iSCSI|S3 (primary copy), local cache for hot data|Extend on-prem storage, only frequently accessed data cached locally|
|**Volume Gateway – Stored**|iSCSI|S3 (async backup), full copy stays on-prem|Local storage is primary, S3 is backup/DR only|
|**Tape Gateway (VTL)**|iSCSI **Virtual Tape Library**|S3 + Glacier / Glacier Deep Archive|Replace physical tape backup infra with virtual tapes|

---

## 1. S3 File Gateway

- Presents a **file interface (NFS/SMB)** on top of S3
- Files are stored as **objects directly in S3**, with a local cache for frequently accessed data (low-latency access to recently used files)
- Supports S3 features: **lifecycle policies, versioning, cross-region replication**
- Great for: **file-based backup, on-prem apps needing file share access to S3 data lakes**

> [!warning] Exam trap S3 File Gateway is for **new** file storage/backup, not full "extend an existing SAN." That's Volume Gateway's job.

---

## 2. Volume Gateway (iSCSI Block Storage)

### Cached Volumes

- **Primary data lives in S3**, only a cache of frequently accessed data is kept locally
- Local storage requirement is much **smaller** (just cache size)
- Low local storage footprint but each read of "cold" data has cloud round-trip latency

### Stored Volumes

- **Primary data lives on-prem** (full copy)
- **Asynchronous, scheduled snapshots** sent to S3 (as EBS snapshots)
- Local storage requirement = full data set size
- Good for: DR backup while keeping on-prem performance for everything

> [!question]- Cached vs Stored — quick decision rule Ask: **"Where does the full/primary copy of data live?"**
> 
> - Primary copy in **AWS** → Cached volumes
> - Primary copy **on-prem** → Stored volumes

### Snapshots

- Both volume types back up to **EBS snapshots** stored in S3
- Snapshots can be used to create actual EBS volumes → easy **migration path** to full cloud (spin up EC2 + EBS from the snapshot)

---

## 3. Tape Gateway (Virtual Tape Library - VTL)

- Presents a **virtual tape library** interface compatible with existing backup software (NetBackup, Veeam, Backup Exec, etc.)
- Virtual tapes are stored in S3, can transition to **Glacier / Glacier Deep Archive** for long-term archival
- Use case: **replace physical tape infrastructure** without changing backup software/workflow
- "Virtual Tape Shelf (VTS)" = the archive; "Virtual Tape Library (VTL)" = active tapes

---

## 4. FSx File Gateway

- Provides **low-latency, on-prem access to Amazon FSx for Windows File Server**
- Caches frequently used data locally, full fidelity to **Windows features**: NTFS ACLs, shadow copies, AD integration
- Use case: branch office needs fast local access to a centrally managed Windows file share in AWS

---

## Deployment options

- [ ] VMware ESXi
- [ ] Microsoft Hyper-V
- [ ] KVM
- [ ] Amazon EC2 (for hybrid testing or as gateway running in-cloud)
- [ ] Hardware appliance purchased directly from AWS

---

## Storage Gateway vs similar services (classic exam confusion)

|Service|Purpose|
|---|---|
|**Storage Gateway**|Bridges **on-prem apps** to AWS storage via standard protocols (ongoing hybrid use)|
|**[[AWS DataSync]]**|**One-time or scheduled bulk data transfer** on-prem ↔ AWS (or AWS ↔ AWS), no ongoing "storage" abstraction|
|**AWS Snowball / Snowball Edge**|**Physical device** for offline bulk transfer when network transfer is too slow|
|**[[Amazon FSx]]**|Fully managed file systems **in AWS** (not a hybrid bridge itself, though FSx File Gateway connects to it)|

> [!tip] Simple rule
> 
> - Need an **ongoing hybrid bridge** with local caching and standard protocols → **Storage Gateway**
> - Need a **fast one-time migration** of files → **DataSync**
> - Network too slow/limited for either → **Snowball family**

---

## Security & integration

- Data encrypted in transit (**SSL/TLS**) and at rest (**SSE-S3 / KMS**)
- Integrates with **IAM** for access control
- CloudWatch metrics for monitoring gateway health and cache performance
- Can run in a VPC via **VPC endpoints** for private connectivity (no public internet needed)

---

## Quick-fire exam facts

- [ ] 4 gateway types: **S3 File, FSx File, Volume (Cached/Stored), Tape (VTL)**
- [ ] S3 File Gateway → objects in S3, NFS/SMB interface
- [ ] Volume Gateway **Cached** → primary in S3, small local cache
- [ ] Volume Gateway **Stored** → primary on-prem, async backup to S3 (as EBS snapshots)
- [ ] Tape Gateway → virtual tapes in S3/Glacier, compatible with existing backup software
- [ ] Snapshots from Volume Gateway can be restored as actual **EBS volumes** — this is the migration trick the exam likes
- [ ] Storage Gateway = ongoing hybrid bridge; **DataSync** = one-time/scheduled transfer; **Snowball** = offline physical transfer

---

## Practice scenario prompts

> [!example] Try these before moving on
> 
> 1. A company wants to eliminate physical tape backup hardware but keep using their existing backup software. → **?**
> 2. An enterprise needs on-prem apps to read/write files that are stored as S3 objects, with lifecycle policies applied. → **?**
> 3. A company wants full on-prem storage performance but needs an off-site backup copy in AWS for DR. → **?**
> 4. A branch office needs fast local access to a Windows file share that's centrally managed in AWS. → **?**

<!-- Answers: 1. Tape Gateway (VTL) 2. S3 File Gateway 3. Volume Gateway - Stored 4. FSx File Gateway -->

---

## Study checklist

- [ ] Memorize all 4 gateway types and their protocols
- [ ] Understand Cached vs Stored volume distinction (where primary data lives)
- [ ] Know that Volume Gateway snapshots become real EBS volumes (migration path)
- [ ] Compare Storage Gateway vs DataSync vs Snowball
- [ ] Understand deployment options (VMware, Hyper-V, KVM, hardware appliance)

#aws/saa #storage-gateway #hybrid-cloud