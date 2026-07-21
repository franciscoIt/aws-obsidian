### Block Storage

Data is split into fixed-size **blocks**, each with its own address, and stored without any file-system context. The OS/application decides how to organize those blocks into files.

**Characteristics:**

- Looks like a raw, unformatted disk to the attached instance
- The instance formats it with a filesystem (NTFS, ext4, etc.) and treats it like a local hard drive
- Typically attached to **one instance at a time** (or a small cluster with special software)
- Very low latency — ideal for databases, boot volumes, transactional apps
- Accessed via protocols like **iSCSI**, or directly as a block device

**AWS example:** Amazon EBS, FSx for NetApp ONTAP (via iSCSI)

### File Storage

Data is stored as **files** inside a hierarchical structure of folders/directories. The storage system itself manages the filesystem — the client just reads/writes files over the network.

**Characteristics:**

- Looks like a shared network drive
- Multiple instances/users can access the **same files simultaneously**
- The storage service manages metadata, permissions, locking, etc.
- Accessed via protocols like **SMB** (Windows) or **NFS** (Linux)

**AWS example:** Amazon FSx for Windows File Server, Amazon EFS

### Quick Analogy

||Block Storage|File Storage|
|---|---|---|
|Think of it as...|An empty hard drive you format yourself|A shared network folder|
|Access unit|Blocks of raw data|Files and folders|
|Simultaneous access|Usually one instance|Many clients at once|
|Protocol|iSCSI, direct attach|SMB, NFS|
|Best for|Databases, OS volumes, clustered apps needing raw disk|Shared documents, home directories, content repositories|

