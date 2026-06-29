- Mounted By instance.
- Bound to an specific availability zone
- persistent data, even after their termination
- Volume. Network drive.
- Delete on termination. Can be enabled.
- bound by az zones

# snapshots 
Features
- archive tier to 75% 
	- taking 24-72h to restore.
- Recycling bin, with specified retention (1day - 1 year)
- Fast snapshot restore (FSR)

# AMI
- Built for specific regions
	- Public or private (ref:aws marketplace) 

# Types
- General purpose SSD (gp2, gp3)
- Provisioned IOPS SSD (io1, io2 block express) --> databases. **Enables EBS multi attach** (up to 16)
- Hard disk drive (st1)  > low cost
- 
![[www.udemy.com_course_aws-certified-solutions-architect-associate-saa-c03_learn_lecture_26098296.png]]
# EBS encryption
It can copy an snapshot of a not encrypted volume and encrypt it 