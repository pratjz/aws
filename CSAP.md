# AWS solutions architect - Professional Exam

## Exam Overview

- Multiple choice and multiple answer questions
- 170 minutes and 75 Questions to complete the exam
- Exam requires a good understanding of all available AWS services
- Passing score is 750 out of 1000 i.e. need to have atleast 60 Questions correct
- Exam fee is USD 300

### What should I bring to an AWS Certification exam?
- Govt ID
- Water Bottle
- You can NOT bring food, laptops, backpacks, notepads, or other personal equipment to the test area. 
- For all exams, you can request a whiteboard and marker (some centers may hand out paper and pencil), which must be returned before you leave. During check in you’ll be asked to turn out your pockets (on jackets, pants, etc.) to verify they’re empty and free of prohibited items. Eyewear will also be inspected to ensure that it’s not technology-enabled.

### On the Exam Day

- Wake up early every day and have a nice breakfast in the morning
- No coffee, try to eat a banana and a cup of orange juice every morning 
- Practice with (https://cloudacademy.com/aws-certifications-training/)
- Drink 5 ltr water every day
- Rewatch the [AWS Solution Architect Professional level certification detailed study-guide with tips and tricks on how to pass the certification](https://cloudacademy.com/webinars/how-study-aws-solutions-architect-professional-certification-24/)
- AWS Youtube channel: watch as many of the technical seminars and re:Invent presentations as you can. Then re-watch. Make notes.
- I would say that your chance of passing the course if you have not have had any practical experience to be seriously compromised. You have no excuse, get an account, login and play.
- Do some sport every day to reset your mind out of work and try to sleep 7h every night
- Once you are feeling confident enough you are ready to take a practice exam [here](https://www.webassessor.com/wa.do?page=publicHome&branding=AMAZON)
- Smile at least once every day

### New Services/concepts that were introduced in the exam: 

- AWS Organizations 
- Managing Organizational Units (OU) 
- Service Control Policies (SCP)
- Difference between SCP and IAM Policy
- AWS Serverless Application Model (AWS SAM) 
- AWS Systems Manager ( and all its sub-services: State Manager, Session Manager, Patch Manager, Automation)
- Cloud Migration
- AWS Config
- AWS Service Catalog
- AWS Application Discovery Service
- AWS Server Migration Service (SMS)
- AWS Rekognition 
- AWS CI/CD Services (CodeCommit, CodeBuild, CodeDeploy, CodePipeline) 

## Key Points to pass the Exam:

### Demonstrate ability to architect the appropriate level of availability based on stakeholder requirements

- Stakeholder requirements is key phrase here – look at what the requirements are first before deciding the best way to architect the solution
- What is availability? Basically up time. Does the customer need 99.99% up time or less? Which products may need to be used to meet this requirement?
- Look at products which are single AZ, multi AZ and multi region. It may be the case that a couple of instances in a single AZ will suffice if cost is a factor
- CloudWatch can be used to perform EC2 or auto scaling actions when status checks fail or metrics are exceeded (alarms, etc)

### Demonstrate ability to implement DR for systems based on RPO and RTO

- What is DR? It is the recovery of systems, services and applications after an unplanned period of downtime.
- What is RPO? Recovery Point Objective. At which point in time do we need to get back to when DR processes are invoked? 3. 3. This would come from a customer requirement – when systems are recovered, data is consistent from 30 minutes prior to the outage, or 1 hour, or 4 hours etc. What is acceptable to the stakeholder?
- What is RTO? Recovery Time Objective. How quickly must systems and services be recovered after invoking DR processes? It may be that all critical systems must be back online within a maximum of four hours.
- RTO and RPO are often paired together to provide an SLA to end users as to when services will be fully restored and how much data may be lost. For example, an RTO of 2 hours and an RPO of 15 minutes would mean all systems would be recovered in two hours or less and consistent to within 15 minutes of the failure.
- How can low RTO be achieved? This can be done by using elastic scaling, for example or using monitoring scripts to power up new instances using the AWS API. You may also use multi AZ services such as EBS and RDS to provide additional resilience
- How can low RPO be achieved? This can be done by using application aware and consistent backup tools, usually native ones such as VSS aware ones from Microsoft or RMAN for Oracle, for example. Databases and real time systems may need to be acquiesced to obtain a crash consistent backup. Standard snapshot tools may not provide this. RMAN can backup to S3 or use point in time snapshots using RDS. RMAN is supported on EC2. Use data dump to move large databases.
- AWS has multi AZ, multi region and services like S3 which has 11 nines of durability with cross region replication
- Glacier – long term archive storage. Cheap but not appropriate for fast recovery (several hours retrieval SLA)
- Storage Gateway is a software appliance that sits on premises that can operate in three modes – gateway cached (hot data kept locally but most data stored in S3), gateway stored (all data kept locally but also replicated to S3) and VTL-Tape Library (virtual disk tapes stored in S3, virtual tape shelf stored in Glacier)
- You should use gateway cached when the requirement is for low cost primary storage with hot data stored locally
- Gateway stored keeps all data locally but takes asynchronous snapshots to S3
- Gateway cached volumes can store 32TB of data, 32 volumes are supported (32 x 32, 1PB)
- Gateway stored volumes are 16TB in size, 12 volumes are supported (16 x 12, 192TB)
- Virtual tape library supports 1500 virtual tapes in S3 (150 TB total)
- Virtual tape shelf is unlimited tapes (uses Glacier)
- Storage Gateway can be on premises or EC2. Can also schedule snapshots, supports Direct Connect and also bandwidth throttling.
- Storage Gateway supports ESXi or Hyper-V, 7.5GB RAM, 75GB storage, 4 or 8 vCPU for installation. To use the Marketplace appliance, you must choose xlarge instance or bigger and m3, i2, c3, c4, r3, d2, or m4 instance types
- Gateway cached requires a separate volume as a buffer upload area and caching area
- Gateway stored requires enough space to hold your full data set and also an upload buffer VTL also requires an upload buffer and cache area
- Ports required for Storage Gateway include 443 (HTTPS) to AWS, port 80 for initial activation only, port 3260 for iSCSI internally and port 53 for DNS (internal)
- Gateway stored snapshots are stored in S3 and can be used to recover data quickly. EBS snapshots can also be used to create a volume to attach to new EC2 instances
- Can also use gateway snapshots to create a new volume on the gateway itself
- Snapshots can also be used to migrate cached volumes into stored volumes, stored volumes into cached volumes and also snapshot a volume to create a new EBS volume to attach to an instance
- Use System Resource Check from the appliance menu to ensure the appliance has enough virtual resources to run (RAM, vCPU, etc.)
- VTL virtual tape retrieval is instantaneous, whereas Tape Shelf (Glacier) can take up to 24 hours
- VTL supports Backup Exec 2012-15, Veeam 7 and 8, NetBackup 7, System Center Data Protection 2012, Dell NetVault 10
- Snapshots can either be scheduled or done ad hoc
- Writes to S3 get throttled as the write buffer gets close to capacity – you can monitor this with CloudWatch
- EBS – Elastic Block Store – block based storage replicated across hosts in a single AZ in a region
- Direct Connect – connection directly into AWS’s data centre via a trusted third party. This can be backed up with standby Direct Connect links or even software VPN
- Route53 also has 100% uptime SLA, Elastic Load Balancing and VPC can also provide a level of resilience if required
- DynamoDB has three copies per region and also can perform multi-region replication
- RDS also supports multi-AZ deployments and read only replicas of data. 5 read only replicas for MySQL, MariaDB and PostGres, 15 for Aurora
- There are four DR models in the AWS white paper:
* Backup and restore (cheap but slow RPO and RTO, use S3 for quick restores and AWS Import/Export for large datasets)
* Pilot Light (minimal replication of the live environment, like the pilot light in a gas heater, it’s used to bring services up with the smallest footprint running in DR. AMIs ready but powered off, brought up manually or by autoscaling
* Data must be replicated to DR from the primary site for failover)
* Warm Standby (again a smaller replication of the live environment but with some services always running to facilitate a quicker failover. It can also be the full complement of servers but running on smaller instances than live. Horizontal scaling is preferred to add more instances to a load balancer)
* Multi-site (active/active configuration where DNS sends traffic to both sites simultaneously. Auto scaling can also add instances for load where required. DNS weighting can be used to route traffic accordingly). DNS weighting is done as a percentage, so if two records have weightings of 10, then the overall is 20 and the percentage is 50% chance of either being used, this is round robin. Weights of 10 and 40 would mean a total of weight 50, with 1 in 5 chance of weight 10 DNS record being used
- Import/Export can import data sets into S3, EBS or Glacier. You can only export from S3
- Import/Export makes sense for large datasets that cannot be moved or copied into AWS over the internet in an efficient manner (time, cost, etc)
- AWS will export data back to you encrypted with TrueCrypt
- AWS will wipe devices after import if specified
- If exporting from an S3 bucket with versioning enabled, only the most recent version is exported
- Encryption for imports is optional, mandatory for exports
- Some services have automated backup:
* RDS
* Redshift
* Elasticache (Redis only)
- EC2 does not have automated backup. You can use either EBS snapshots or create an AMI Image from a running or stopped instance. The latter option is especially useful if you have an instance storage on the host which is ephemeral and will get deleted when the instance is stopped (Bundle Instance). You can “copy” the host storage for the instance by creating an AMI, which can then be copied to another region
- To restore a file on a server for example, take regular snapshots of the EBS volume, create a volume from the snapshot, mount the volume to the instance, browse and recover the files as necessary
- MySQL requires InnoDB for automated backups, if you delete an instance then all automated backups are deleted, manual DB snapshots stored in S3 are not deleted
- All backups are stored in S3
- When you do an RDS restore, you can change the engine type (SQL Standard to Enterprise, for example), assuming you have enough storage space.
- Elasticache automated backups snapshot the whole cluster, so there will be performance degradation whilst this takes place. Backups are stored on S3.
- Redshift backups are stored on S3 and have a 1 day retention period by default and only backs up delta changes to keep storage consumption to a minimum
- EC2 snapshots are stored in S3 and are incremental and each snapshot still contains the base snapshot data. You are only charged for the incremental snapshot storage

### Determine appropriate use of multi-Availability Zones vs. multi-Region architectures

- Multi-AZ services examples are S3, RDS, DynamoDB. Using multi-AZ can mitigate against the loss of up to two AZs (data centres, assuming there are three. Some regions only have two). This can provide a good balance between cost, complexity and reliability
- Multi-region services can mitigate failures in AZs or individual regions, but may cost more and introduce more infrastructure and complexity. Use ELB for multi-region failover and resilience, CloudFront
- DynamoDB offers cross region replication, RDS offers the ability to snapshot from one region to another to have read only replicas. Code Pipeline has a built in template for replicating DynamoDB elsewhere for DR
- Redshift can snapshot within the same region and also replicate to another region

### Demonstrate ability to implement self-healing capabilities

- HA available already for most popular databases:-
- SQL Server Availability Groups, SQL Mirroring, log shipping. Read replicas in other AZs not supported
- MySQL – Asynchronous mirroring
- Oracle – Data Guard, RAC (RAC not supported on AWS but can run on EC2 by using VPN and Placement Groups as multicast is not supported)
- RDS has multi-AZ automatic failover to protect against
- Loss of availability in primary AZ
- Loss of connectivity to primary DB
- Storage or host failure of primary DB
- Software patching (done by AWS, remember)
- Rebooting of primary DB
- Uses master and slave model
- MySQL, Oracle and Postgres use physical layer replication to keep data consistent on the standby instance
- SQL Server uses application layer mirroring but achieves the same result
- Multi-AZ uses synchronous replication (consistent read/write), asynchronous (potential data loss) is only used for read replicas
- DB backups are taken from the secondary to reduce I/O load on the primary
- DB restores are taken from the secondary to avoid I/O suspension on the primary
- AZ failover can be forced by rebooting your instance either via the console or via the RebootDBInstance API call
- Multi-AZ databases are used for DR, not as a scaling solution. Scale can be achieved by using read replicas, this can be done via the AWS console or by using the CreateDBInstanceReadReplica API call
- Amazon Aurora employs a highly durable, SSD-backed virtualized storage layer purpose-built for database workloads. 
- Amazon Aurora automatically replicates your volume six ways, across three Availability Zones. Amazon Aurora storage is fault-tolerant, transparently handling the loss of up to two copies of data without affecting database write availability and up to three copies without affecting read availability. Amazon Aurora storage is also self-healing. Data blocks and disks are continuously scanned for errors and replaced automatically.
- Creating a read replica means a snapshot of your primary DB instance, this may result in a pause of about a minute in non multi-AZ deployments
- Multi-AZ deployments will use a secondary for a snapshot
- A new DNS endpoint address is given for the read only replica, you need to update the app
- You can promote a read only replica to be a standalone, but this breaks replication
- MySQL and Postgres can have up to 5 replicas
- Read replicas in different regions for MySQL only
- Replication is asynchronous only
- Read replicas can be built off Multi-AZ databases
- Read replicas are not multi-AZ
- MySQL can have read replicas of read replicas, but this increases latency
- DB Snapshots and automated backups cannot be taken of read replicas
- Consider using DynamoDB instead of RDS if your database does not require:-
- Transaction support
- Atomicity
- Consistency
- Isolation
- Durability
- ACID (durability) compliance
- Joins
- SQL

## General Learning Material

- <a href="https://cloudacademy.com/learning-paths/solutions-architect-professional-aws-17/">Solutions Architect—  Professional Certification for AWS (2016)</a>
- <a href="https://cloudacademy.com/ebooks/guide-aws-certification-exams-2/">A Guide to AWS Certification Exams</a>
- <a href="https://www.dropbox.com/s/hizoeicmgf4iha5/DDoS_White_Paper_June2015.pdf">AWS Best Practices for DDoS Resiliency</a>
- <a href="https://aws.amazon.com/kinesis/streams/faqs/">Amazon Kinesis Streams FAQs</a>
- <a href="https://docs.aws.amazon.com/streams/latest/dev/kinesis-dg.pdf">Amazon Kinesis Streams: Developer Guide</a>
- <a href="http://docs.aws.amazon.com/IAM/latest/UserGuide/iam-ug.pdf">AWS Identity and Access Management User Guide</a>
- <a href="https://aws.amazon.com/it/cloudfront/dynamic-content/">Amazon CloudFront - Dynamic Content Delivery</a>
- <a href="https://aws.amazon.com/it/redshift/faqs/">Amazon Redshift FAQs</a>
- <a href="http://cloudacademy.com/blog/amazon-aws-certified-solutions-architect-what-to-study-tips-and-resources/">Amazon AWS Certified Solutions Architect: What to Study, Tips and Resources</a>
- <a href="http://nineofclouds.blogspot.ch/2013/01/vpc-migration-nats-bandwidth-bottleneck.html">VPC Migration: NATs & Bandwidth Bottleneck</a>
- <a href="https://docs.aws.amazon.com/sns/latest/dg/sns-dg.pdf">Amazon Simple Notification Service Developer Guide</a>
- <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/s3-dg.pdf">Amazon Simple Storage Service Developer Guide</a>
- <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-ug.pdf">Amazon Virtual Private Cloud User Guide</a>
- <a href="http://d0.awsstatic.com/whitepapers/migration-best-practices-rdbms-to-dynamodb.pdf">Best Practices for Migrating from RDBMS to Amazon DynamoDB</a>
- <a href="http://cloudacademy.com/blog/aws-certification-exams-what-to-expect/">AWS Certification Exams: What to expect at the Exam</a>
- <a href="http://blogs.aws.amazon.com/security/post/Tx71TWXXJ3UI14/Enabling-Federation-to-AWS-using-Windows-Active-Directory-ADFS-and-SAML-2-0">Enabling Federation to AWS Using Windows Active Directory, ADFS, and SAML 2.0</a>
- <a href="https://d0.awsstatic.com/Train%20&%20Cert/docs/AWS_certified_solutions_architect_professional_examsample.pdf">AWS Certified Solutions Architect – Professional Level Sample Exam Questions</a>
- <a href="https://d0.awsstatic.com/training-and-certification/docs-sa-pro/AWS_certified_solutions_architect_professional_blueprint.pdf">AWS Certified Solutions Architect Professional Exam Blueprint</a>
- <a href="https://www.youtube.com/watch?v=iJeiWU9Ud7w">Cloud Architectures with AWS Direct Connect (ARC304) | AWS re:Invent 2013</a>
- <a href="https://www.youtube.com/watch?v=Ut5TG1ueU1E">AWS re: Invent STG 204: Using AWS Storage Gateway</a>
- <a href="https://www.youtube.com/watch?v=k32FaqM40rw">Storage TCO using AWS Storage Gateway, Amazon S3 and Amazon Glacier (STG202) | AWS re:Invent 2013</a>
- <a href="https://www.youtube.com/watch?v=wioDkHQ9Pm8&index=10&list=PLhr1KZpdzuketO0VQtLwchbqJLO4vDjAq">AWS re:Invent 2014 | (ARC206) Architecting Reactive Applications on AWS (Playlist)</a>
- <a href="https://www.youtube.com/watch?v=n6eL9W1Go08">AWS June Webinar Series - Deep dive: Hybrid Architectures</a>
- <a href="https://d0.awsstatic.com/training-and-certification/docs-sa-pro/AWS_certified_solutions_architect_professional_blueprint.pdf">AWS January 2016 Webinar Series - Managing your Infrastructure as Code</a>
- <a href="https://iam.cloudonaut.io/">Complete AWS IAM Reference</a>
- <a href="https://cloudonaut.io/templates-for-aws-cloudformation/">Free Templates for AWS CloudFormation</a>
- <a href="https://cloudacademy.com/webinars/how-study-aws-solutions-architect-professional-certification-24/">How to study for the AWS Solutions Architect Professional Certification (Webinar)</a>
- <a href="https://soundcloud.com/cloudacademyinc/cloud-academy-office-hours-aws-certifications-special-guest">Interview with 5 AWS Certified Greg Cockburn (Podcast)</a>
- <a href="https://soundcloud.com/cloudacademyinc/cloud-academy-office-hours-special-guest">Interview with 5 AWS Certified Stephen Wilding (Podcast)</a>
- Cloud Migration: https://aws.amazon.com/cloud-migration/
- AWS Organizations & SCP: https://aws.amazon.com/organizations/getting-started/
- SCP vs IAM Policy: https://aws.amazon.com/premiumsupport/knowledge-center/iam-policy-service-control-policy/
- AWS Systems Manager: https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html
- Serverless: https://aws.amazon.com/serverless/
- Access Management: https://docs.amazonaws.cn/en_us/IAM/latest/UserGuide/access.html
- Web application hosting in AWS: https://d0.awsstatic.com/whitepapers/aws-web-hosting-best-practices.pdf
- AWS Disaster Recovery: http://d36cz9buwru1tt.cloudfront.net/AWS_Disaster_Recovery.pdf
