<h2>AWS solutions architect - Professional Exam</h2>

<h3>Exam Overview</h3>

1. Multiple choice and multiple answer questions
2. 170 minutes and 75 Questions to complete the exam
7. Exam requires a good understanding of all available AWS services
8. Passing score is 80% i.e. need t ohave atleast 60 Questions correct
9. Exam fee is USD 300

<h3>What should I bring to an AWS Certification exam?</h3>
- Govt ID
- Water Bottle
- You can NOT bring food, laptops, backpacks, notepads, or other personal equipment to the test area. For all exams, you can request a whiteboard and marker (some centers may hand out paper and pencil), which must be returned before you leave. During check in you’ll be asked to turn out your pockets (on jackets, pants, etc.) to verify they’re empty and free of prohibited items. Eyewear will also be inspected to ensure that it’s not technology-enabled.

<h2>The Exam Day</h2>

<b>Just a couple of suggestions to pass the AWS Solutions Architect Professional level certification:</b>

1. Wake up early every day and have a nice breakfast in the morning
2. No coffee, try to eat a banana and a cup of orange juice every morning 
3. Practice with [our specific resources for the AWS Certifications](https://cloudacademy.com/aws-certifications-training/)
4. Drink water every day
5. Get the most out of [this GitHub repository](https://gist.github.com/leonardofed/bbf6459ad154ad5215d354f3825435dc)
6. Rewatch the [AWS Solution Architect Professional level certification detailed study-guide with tips and tricks on how to pass the certification](https://cloudacademy.com/webinars/how-study-aws-solutions-architect-professional-certification-24/)
8. AWS Youtube channel: watch as many of the technical seminars and re:Invent presentations as you can. Then re-watch. Make notes.
9. I would say that your chance of passing the course if you have not have had any practical experience to be seriously compromised. You have no excuse, get an account, login and play.
10. Do some sport every day to reset your mind out of work and try to sleep 7h every night 
11. Repeat
12. Once you are feeling confident enough you are ready to take a practice exam [here](https://www.webassessor.com/wa.do?page=publicHome&branding=AMAZON)
13. Smile at least once every day 
14. Have fun

<h2>Key Points to pass the Exam:</h2>
<h3>Demonstrate ability to architect the appropriate level of availability based on stakeholder requirements</h3>

1. Stakeholder requirements is key phrase here – look at what the requirements are first before deciding the best way to architect the solution
2. What is availability? Basically up time. Does the customer need 99.99% up time or less? Which products may need to be used to meet this requirement?
3. Look at products which are single AZ, multi AZ and multi region. It may be the case that a couple of instances in a single AZ will suffice if cost is a factor
4. CloudWatch can be used to perform EC2 or auto scaling actions when status checks fail or metrics are exceeded (alarms, etc)

<h3>Demonstrate ability to implement DR for systems based on RPO and RTO</h3>

1. What is DR? It is the recovery of systems, services and applications after an unplanned period of downtime.
2. What is RPO? Recovery Point Objective. At which point in time do we need to get back to when DR processes are invoked? 3. 3. This would come from a customer requirement – when systems are recovered, data is consistent from 30 minutes prior to the outage, or 1 hour, or 4 hours etc. What is acceptable to the stakeholder?
4. What is RTO? Recovery Time Objective. How quickly must systems and services be recovered after invoking DR processes? It may be that all critical systems must be back online within a maximum of four hours.
5. RTO and RPO are often paired together to provide an SLA to end users as to when services will be fully restored and how much data may be lost. For example, an RTO of 2 hours and an RPO of 15 minutes would mean all systems would be recovered in two hours or less and consistent to within 15 minutes of the failure.
6. How can low RTO be achieved? This can be done by using elastic scaling, for example or using monitoring scripts to power up new instances using the AWS API. You may also use multi AZ services such as EBS and RDS to provide additional resilience
7. How can low RPO be achieved? This can be done by using application aware and consistent backup tools, usually native ones such as VSS aware ones from Microsoft or RMAN for Oracle, for example. Databases and real time systems may need to be acquiesced to obtain a crash consistent backup. Standard snapshot tools may not provide this. RMAN can backup to S3 or use point in time snapshots using RDS. RMAN is supported on EC2. Use data dump to move large databases.
8. AWS has multi AZ, multi region and services like S3 which has 11 nines of durability with cross region replication
9. Glacier – long term archive storage. Cheap but not appropriate for fast recovery (several hours retrieval SLA)
19. Storage Gateway is a software appliance that sits on premises that can operate in three modes – gateway cached (hot data kept locally but most data stored in S3), gateway stored (all data kept locally but also replicated to S3) and VTL-Tape Library (virtual disk tapes stored in S3, virtual tape shelf stored in Glacier)
11. You should use gateway cached when the requirement is for low cost primary storage with hot data stored locally
12. Gateway stored keeps all data locally but takes asynchronous snapshots to S3
13. Gateway cached volumes can store 32TB of data, 32 volumes are supported (32 x 32, 1PB)
14. Gateway stored volumes are 16TB in size, 12 volumes are supported (16 x 12, 192TB)
15. Virtual tape library supports 1500 virtual tapes in S3 (150 TB total)
16. Virtual tape shelf is unlimited tapes (uses Glacier)
17. Storage Gateway can be on premises or EC2. Can also schedule snapshots, supports Direct Connect and also bandwidth throttling.
18. Storage Gateway supports ESXi or Hyper-V, 7.5GB RAM, 75GB storage, 4 or 8 vCPU for installation. To use the Marketplace appliance, you must choose xlarge instance or bigger and m3, i2, c3, c4, r3, d2, or m4 instance types
19. Gateway cached requires a separate volume as a buffer upload area and caching area
20. Gateway stored requires enough space to hold your full data set and also an upload buffer
VTL also requires an upload buffer and cache area
21. Ports required for Storage Gateway include 443 (HTTPS) to AWS, port 80 for initial activation only, port 3260 for iSCSI internally and port 53 for DNS (internal)
22. Gateway stored snapshots are stored in S3 and can be used to recover data quickly. EBS snapshots can also be used to create a volume to attach to new EC2 instances
23. Can also use gateway snapshots to create a new volume on the gateway itself
24. Snapshots can also be used to migrate cached volumes into stored volumes, stored volumes into cached volumes and also snapshot a volume to create a new EBS volume to attach to an instance
25. Use System Resource Check from the appliance menu to ensure the appliance has enough virtual resources to run (RAM, vCPU, etc.)
26. VTL virtual tape retrieval is instantaneous, whereas Tape Shelf (Glacier) can take up to 24 hours
27. VTL supports Backup Exec 2012-15, Veeam 7 and 8, NetBackup 7, System Center Data Protection 2012, Dell NetVault 10
28. Snapshots can either be scheduled or done ad hoc
29. Writes to S3 get throttled as the write buffer gets close to capacity – you can monitor this with CloudWatch
30. EBS – Elastic Block Store – block based storage replicated across hosts in a single AZ in a region
31. Direct Connect – connection directly into AWS’s data centre via a trusted third party. This can be backed up with standby Direct Connect links or even software VPN
32. Route53 also has 100% uptime SLA, Elastic Load Balancing and VPC can also provide a level of resilience if required
32. DynamoDB has three copies per region and also can perform multi-region replication
33. RDS also supports multi-AZ deployments and read only replicas of data. 5 read only replicas for MySQL, MariaDB and PostGres, 15 for Aurora
34. There are four DR models in the AWS white paper:
 * Backup and restore (cheap but slow RPO and RTO, use S3 for quick restores and AWS Import/Export for large datasets)
 * Pilot Light (minimal replication of the live environment, like the pilot light in a gas heater, it’s used to bring services up with the smallest footprint running in DR. AMIs ready but powered off, brought up manually or by autoscaling
 * Data must be replicated to DR from the primary site for failover)
 * Warm Standby (again a smaller replication of the live environment but with some services always running to facilitate a quicker failover. It can also be the full complement of servers but running on smaller instances than live. Horizontal scaling is preferred to add more instances to a load balancer)
 * Multi-site (active/active configuration where DNS sends traffic to both sites simultaneously. Auto scaling can also add instances for load where required. DNS weighting can be used to route traffic accordingly). DNS weighting is done as a percentage, so if two records have weightings of 10, then the overall is 20 and the percentage is 50% chance of either being used, this is round robin. Weights of 10 and 40 would mean a total of weight 50, with 1 in 5 chance of weight 10 DNS record being used
35. Import/Export can import data sets into S3, EBS or Glacier. You can only export from S3
36. Import/Export makes sense for large datasets that cannot be moved or copied into AWS over the internet in an efficient manner (time, cost, etc)
37. AWS will export data back to you encrypted with TrueCrypt
38. AWS will wipe devices after import if specified
39. If exporting from an S3 bucket with versioning enabled, only the most recent version is exported
40. Encryption for imports is optional, mandatory for exports
41. Some services have automated backup:
 * RDS
 * Redshift
 * Elasticache (Redis only)
42. EC2 does not have automated backup. You can use either EBS snapshots or create an AMI Image from a running or stopped instance. The latter option is especially useful if you have an instance storage on the host which is ephemeral and will get deleted when the instance is stopped (Bundle Instance). You can “copy” the host storage for the instance by creating an AMI, which can then be copied to another region
43. To restore a file on a server for example, take regular snapshots of the EBS volume, create a volume from the snapshot, mount the volume to the instance, browse and recover the files as necessary
44. MySQL requires InnoDB for automated backups, if you delete an instance then all automated backups are deleted, manual DB snapshots stored in S3 are not deleted
45. All backups are stored in S3
When you do an RDS restore, you can change the engine type (SQL Standard to Enterprise, for example), assuming you have enough storage space.
46. Elasticache automated backups snapshot the whole cluster, so there will be performance degradation whilst this takes place. Backups are stored on S3.
46. Redshift backups are stored on S3 and have a 1 day retention period by default and only backs up delta changes to keep storage consumption to a minimum
47. EC2 snapshots are stored in S3 and are incremental and each snapshot still contains the base snapshot data. You are only charged for the incremental snapshot storage

<h3>Determine appropriate use of multi-Availability Zones vs. multi-Region architectures</h3>

1. Multi-AZ services examples are S3, RDS, DynamoDB. Using multi-AZ can mitigate against the loss of up to two AZs (data centres, assuming there are three. Some regions only have two). This can provide a good balance between cost, complexity and reliability
2. Multi-region services can mitigate failures in AZs or individual regions, but may cost more and introduce more infrastructure and complexity. Use ELB for multi-region failover and resilience, CloudFront
3. DynamoDB offers cross region replication, RDS offers the ability to snapshot from one region to another to have read only replicas. Code Pipeline has a built in template for replicating DynamoDB elsewhere for DR
4. Redshift can snapshot within the same region and also replicate to another region

<h3>Demonstrate ability to implement self-healing capabilities</h3>

1. HA available already for most popular databases:-
2. SQL Server Availability Groups, SQL Mirroring, log shipping. Read replicas in other AZs not supported
3. MySQL – Asynchronous mirroring
4. Oracle – Data Guard, RAC (RAC not supported on AWS but can run on EC2 by using VPN and Placement Groups as multicast is not supported)
5. RDS has multi-AZ automatic failover to protect against
6. Loss of availability in primary AZ
7. Loss of connectivity to primary DB
8. Storage or host failure of primary DB
9. Software patching (done by AWS, remember)
10. Rebooting of primary DB
11. Uses master and slave model
12. MySQL, Oracle and Postgres use physical layer replication to keep data consistent on the standby instance
13. SQL Server uses application layer mirroring but achieves the same result
14. Multi-AZ uses synchronous replication (consistent read/write), asynchronous (potential data loss) is only used for read replicas
15. DB backups are taken from the secondary to reduce I/O load on the primary
16. DB restores are taken from the secondary to avoid I/O suspension on the primary
17. AZ failover can be forced by rebooting your instance either via the console or via the RebootDBInstance API call
18. Multi-AZ databases are used for DR, not as a scaling solution. Scale can be achieved by using read replicas, this can be done via the AWS console or by using the CreateDBInstanceReadReplica API call
19. Amazon Aurora employs a highly durable, SSD-backed virtualized storage layer purpose-built for database workloads. 
20. Amazon Aurora automatically replicates your volume six ways, across three Availability Zones. Amazon Aurora storage is fault-tolerant, transparently handling the loss of up to two copies of data without affecting database write availability and up to three copies without affecting read availability. Amazon Aurora storage is also self-healing. Data blocks and disks are continuously scanned for errors and replaced automatically.
21. Creating a read replica means a snapshot of your primary DB instance, this may result in a pause of about a minute in non multi-AZ deployments
22. Multi-AZ deployments will use a secondary for a snapshot
23. A new DNS endpoint address is given for the read only replica, you need to update the app
24. You can promote a read only replica to be a standalone, but this breaks replication
25. MySQL and Postgres can have up to 5 replicas
26. Read replicas in different regions for MySQL only
26. Replication is asynchronous only
27. Read replicas can be built off Multi-AZ databases
28. Read replicas are not multi-AZ
29. MySQL can have read replicas of read replicas, but this increases latency
30. DB Snapshots and automated backups cannot be taken of read replicas
31. Consider using DynamoDB instead of RDS if your database does not require:-
32. Transaction support
33. Atomicity
34. Consistency
35. Isolation
36. Durability
37. ACID (durability) compliance
38. Joins
39. SQL

<i><a href="https://blue-clouds.com/2016/06/21/21-06-16/">Credits to Chris Beckett @ BlueClouds</a>

<h2>General Learning Material</h2>
To prepare at best for the exam you should start with an overview of the concepts and knowledge areas covered on the exam and walks you through the exam structure and question formats. Get an hands-on practice with advanced use cases, while practice exam questions test your understanding of key architectural concepts. 

1. <a href="https://cloudacademy.com/learning-paths/solutions-architect-professional-aws-17/">Solutions Architect—  Professional Certification for AWS (2016)</a>
2. <a href="https://cloudacademy.com/ebooks/guide-aws-certification-exams-2/">A Guide to AWS Certification Exams</a>
3. <a href="https://www.dropbox.com/s/hizoeicmgf4iha5/DDoS_White_Paper_June2015.pdf">AWS Best Practices for DDoS Resiliency</a>
4. <a href="https://aws.amazon.com/kinesis/streams/faqs/">Amazon Kinesis Streams FAQs</a>
5. <a href="https://docs.aws.amazon.com/streams/latest/dev/kinesis-dg.pdf">Amazon Kinesis Streams: Developer Guide</a>
6. <a href="http://docs.aws.amazon.com/IAM/latest/UserGuide/iam-ug.pdf">AWS Identity and Access Management User Guide</a>
6. <a href="https://aws.amazon.com/it/cloudfront/dynamic-content/">Amazon CloudFront - Dynamic Content Delivery</a>
7. <a href="https://aws.amazon.com/it/redshift/faqs/">Amazon Redshift FAQs</a>
8. <a href="http://cloudacademy.com/blog/amazon-aws-certified-solutions-architect-what-to-study-tips-and-resources/">Amazon AWS Certified Solutions Architect: What to Study, Tips and Resources</a>
9. <a href="http://nineofclouds.blogspot.ch/2013/01/vpc-migration-nats-bandwidth-bottleneck.html">VPC Migration: NATs & Bandwidth Bottleneck</a>
10. <a href="https://docs.aws.amazon.com/sns/latest/dg/sns-dg.pdf">Amazon Simple Notification Service Developer Guide</a>
11. <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/s3-dg.pdf">Amazon Simple Storage Service Developer Guide</a>
12. <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-ug.pdf">Amazon Virtual Private Cloud User Guide
</a>
13. <a href="http://d0.awsstatic.com/whitepapers/migration-best-practices-rdbms-to-dynamodb.pdf">Best Practices for Migrating from RDBMS to Amazon DynamoDB</a>
14. <a href="http://cloudacademy.com/blog/aws-certification-exams-what-to-expect/">AWS Certification Exams: What to expect at the Exam
</a>
15. <a href="http://blogs.aws.amazon.com/security/post/Tx71TWXXJ3UI14/Enabling-Federation-to-AWS-using-Windows-Active-Directory-ADFS-and-SAML-2-0">Enabling Federation to AWS Using Windows Active Directory, ADFS, and SAML 2.0</a>
16. <a href="https://d0.awsstatic.com/Train%20&%20Cert/docs/AWS_certified_solutions_architect_professional_examsample.pdf">AWS Certified Solutions Architect – Professional Level Sample Exam Questions</a>
17. <a href="https://d0.awsstatic.com/training-and-certification/docs-sa-pro/AWS_certified_solutions_architect_professional_blueprint.pdf">AWS Certified Solutions Architect Professional Exam Blueprint
</a>
18. <a href="https://www.youtube.com/watch?v=iJeiWU9Ud7w">Cloud Architectures with AWS Direct Connect (ARC304) | AWS re:Invent 2013</a>
19. <a href="https://www.youtube.com/watch?v=Ut5TG1ueU1E">AWS re: Invent STG 204: Using AWS Storage Gateway</a>
20. <a href="https://www.youtube.com/watch?v=k32FaqM40rw">Storage TCO using AWS Storage Gateway, Amazon S3 and Amazon Glacier (STG202) | AWS re:Invent 2013</a>
21. <a href="https://www.youtube.com/watch?v=wioDkHQ9Pm8&index=10&list=PLhr1KZpdzuketO0VQtLwchbqJLO4vDjAq">AWS re:Invent 2014 | (ARC206) Architecting Reactive Applications on AWS (Playlist)</a>
22. <a href="https://www.youtube.com/watch?v=n6eL9W1Go08">AWS June Webinar Series - Deep dive: Hybrid Architectures</a>
23. <a href="https://d0.awsstatic.com/training-and-certification/docs-sa-pro/AWS_certified_solutions_architect_professional_blueprint.pdf">AWS January 2016 Webinar Series - Managing your Infrastructure as Code</a>
24. <a href="https://iam.cloudonaut.io/">Complete AWS IAM Reference</a>
25. <a href="https://cloudonaut.io/templates-for-aws-cloudformation/">Free Templates for AWS CloudFormation</a>
26. <a href="https://cloudacademy.com/webinars/how-study-aws-solutions-architect-professional-certification-24/">How to study for the AWS Solutions Architect Professional Certification (Webinar)</a>
27. <a href="https://soundcloud.com/cloudacademyinc/cloud-academy-office-hours-aws-certifications-special-guest">Interview with 5 AWS Certified Greg Cockburn (Podcast)</a>
28. <a href="https://soundcloud.com/cloudacademyinc/cloud-academy-office-hours-special-guest">Interview with 5 AWS Certified Stephen Wilding (Podcast)</a>
<br>

