# Chapter 1: Introduction

- New services must be at least 6 months old to make it into the exam
- **Gap from Associate to Professional is really large
- Quickly Move from understand and remember to evaulate and apply
- Study to become and expert not just to pass the exam

Course Structure
1. Architectural Concepts
2. Service and Capabilities
3. Additional Resources
4. Exam Summary
5. Pro Tips
6. Domain Challenges and Walkthrough
7. Quiz

# Data Stores

## Concepts

Persistent | Transient | Ephemeral
-|-|-
Glacier,RDS|SQS SNS Kinesis| EC2 Instance Store, Memcached

IPOS vs Throughput

**IPOS** - read write speed

**Throughput** - how much data can we move at one time

Consistency Models

**ACID** atomic consistent isolated durable (RDS)

**BASE** basic availability even if its not always consistent - eventually consistent (S3 dynamoDB)

## Storage
### S3
- One of the first services after SQS
- used in alot of aws services
- max object size 5 TB
- max PUT request is 5GB
- use multipart upload for more than 100MB

URL IS NOT A PATH ITS A KEY

think of S3 as a multi AZ database

#### Consistency
Read after Write Consistency for new PUT - waits until new object is replicated then return 200

Eventual Consistency for HEAD or GET of exisitng objects - may be state

Eventual Consistency for PUT and DELETES

Updates to a single key are atomic

#### Security
1. Resource based - Object ACL and Bucket Policy
2. User based - IAM Policies and role
3. MFA before Delete

#### Data Protection
1. MFA before Delete
2. Versioning
3. Cross Region Replication

#### Life Cycle Management
Apply life cycle rules for your resources

#### Analytics
Data Lake Concept w Redshift 
IOT Streaming data w Kinesis Firehose
ML and AL Storage w Lex
Storage Class Analysis w S3 Management Analytics

#### Encryption

At Rest

Option|Implementation
-|-
SSE-S3|use S3's key
SSE-C| upload your own key
SSE-KMS| use a key from KMS
Client-Side|store encrypted objects

In Flight - SSL

#### Cool Tricks
**Tranfser  Acceleration** Reverse CloudFront
**Requester Pays** - requester pays rather than th bucket owner
**Tags** tags are especially useful in S3
**Event** can emit and trigger events to other services
**Static Web Hosting** host with massive scalability
**BitTorrent** can use S3 to torrent files woah

### Glacier
- achiving service
- cold/offline service
- Used by VTL(Virtual Tape Library)
- Faster retrival if you pay more
- can be integrated with S3 lifecycle management
- **IMMUTABLEEEEEEEEEEEE**

Terms:
- Vaults => S3 Bucket
- Archive => S3 Object
- Vault Lock => enables contraints eg no delete

Access is granted via IAM

Limits: 40 TB

Vault Locks ->  initiate -> complete the lock within 24 hours -> after that it cant be undone

### Elastic Block Storage
- virutal harddrives
- can only be used by EC2
- tied to a single AZ (impt)
- pay for the entire volume

can optimize by IOPS and throughput and cost

EBS vs Instant Stores

Instant Store: fast IO but temp
EBS: snapshots and persistence

Snapshots
- backup
- share volumes with others
- migrate volume to new AZ
- convert unencrypted to encrypted
- only changes are changed (like a commit history) makes it cost effective - IMPT aws magic

### Elastic File Service
- NFS - Network File Service
- Implements NFS
- elastic - only pay for what you use
- 3x more expensive than EFS
- 20x more expensive than S3
- Configure multiple mount points
- Can be mounted on premise only with Direct Connect

**EFS File Sync Agent**
sync files in the background

Checkout not supported NFSv4 features...

### Storage Gateway
run on
- onsite as a VM
- EC2

sync on premise data to the cloud
useful in cloud migration

Modes

New Name|Old Name|Interface|Function
-|-|-|-
File Gateway|None|NFS,SMB|Allow on premise to store files on S3 via a NFS mount point
Volume Gateway Stored Mode|Gateway-stored volumes|iSCSI|sync on site data with S3
Volume Gateway Cached Mode|Gateway Cached Mode|iSCSI|only stored in S3 and cached locally
Tape Gateway|Gateway Virtal Tape Library|iSCSI|supports tape machines backup to glacier

**Bandwidth Throttleing**
use caching to throttleing

### WorkDocs
- secure fully managed file collaboration service
- Can integrate with AD for SSO
- complient

## Databases

### EC2 Database
- if you need a DB is there is no AWS s service
- can run any kind of DB you want

use case: SAP HANA not a managed service on AWS but can run on an EC2 instance

### Relaional Database Service (RDS)
RDS Anti-Patterns
- large binary or blobs
- automated scalability
- name valued pair
- not well structured/no fixed schema
- other db not supported eg HANA or DB2
- complete control over the databsae -> use EC2


supported databases -> MySQL, MariaDB, PostgreSQL, Oracle, and Microsoft SQL Server DB engines

**Replication** only supported by innodb engine. (all)

Master Slave replication

**Speical Note**
Multi AZ mater/slave -  replication is sync
Cross Region read replica - replication is async (eventually consistent)


Multi AZ Master/ Cross Region Read Replica
-|-
automatically promoted to a master if the master goes down|can be promoted manually
replication is sync|replication is async (eventually consistent)

### DynamoDB
- multi AZ, massivly scalable noSQL key value store
- usually consisitent under a sec (eventual consistent)
- cross region replicaitons
- can request a strongly consistent read -ve HA
- BASE over ACID
- but also can do ACID transaction
- priced on throughput and tranactions rather than compute
- autoscalling -> scales up first but scale down slow


**DynamoDB Transaction**
now got overlap with RDS

**Special Values**
collection -> tabls
table -> store items
item -> consists of attributes
attribute -> key value pair
primary key -> hashed to get a unique key (must be unique)

index -> sort key  + partition key
sort key -> stored in this sorted order (used for indexing)
partition key -> has be unique

Secondary Indexes
global - partition and sort key can be different
local - partition key must be the same

Limits 
- 5 local and 5 global secondary indexes
- max 20 attributes
- indexes take up storage space

why bother with keys
- speed up lookup

global - fast query of attributes outside the pk
local - 

secondary indexs -  like a view from rds

dont really understand...

### RedShift
- fully managed clustered peta byte scale data warehouse
- postgress SQL compatible
- can work with SQL tools

**RedShift Spectrum** allows you to work with data files directly from S3

**Data Lake** large collection of data. 

Redshift is the framework to make sense of large set of data using SQL to query it.

### Neptune
Fully managed Graph Databases
relationships between objects
vertices and edges

dont think its tested

### ElastiCache
Fully managed in memory data stores
redis and memached
rally really fast 
not persitence (by and large)
priced on node size and hours of usage

use case 
- store session data for scability
- leaderboards
- landing spot for dashboards


Redis|Memcached
-|-
Complex|Simple
cant scale horizontally|can scale horizontally
can do read replicas and snapshot | cant do anything
single threaded| multi threaded by deafult

auto discovery...

### Aurora
automated failover to replicas


## Comparing Databases

EC2
- ultimate control
- when prefered db no on RDS
RDS
- need a traditional db for LTP
- well formed data
DynamoDB
- name value pair
- in memory performace with persistence
RedShift
- massive amounts of daa
- primary for OLAP workloads
Neptune
- graph database to model relationships between objects
Elastic Cache
- Fast temp storage for small amounts of data
- highly volatile data

read whitepaper AWS Storage Options and take note of anti patterns

## Pro Tips

Cheaper to run DB on EC2
but must consider intagibles

performace hit when RDS backups run in a single AZ deployment
youll want to backup off a standby or a replica

## Challenge 1
DBAC 
ans:DACB
## Challenge 2


## Additional Resources

https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf

https://d1.awsstatic.com/whitepapers/Multi_Tenant_SaaS_Storage_Strategies.pdf

https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

https://www.youtube.com/watch?v=SUWqDOnXeDw

https://www.youtube.com/watch?v=TJxC-B9Q9tQ

https://www.youtube.com/watch?v=9wgaV70FeaM

# Chapter 3: Networking

Learn the OSI model

AWS disables multicastvand broadcast

Ephermeral Ports - above 1024

source dest check - aws only allows an ec2 to send traffic if it is the source or dest
need to disable this for NAT gateway

physical AZ locations are assign to a user when created
can differ from other
US-EAST-1 != someone elses US-EAST-1

**AWS Managed VPN**
- runs over IPSec
- can be used as a redundant connection to aws 
- dependent on internet connection
- static routes or BGP for dynamic routing

**Direct Connect**
Consistent and predictable
up to 10 GB
on site router to aws outer
work with your networking provider
connect to your aws resources via Virtual Interfaces (VIF)

Secondary connection to reduce SPF
can run an additional IPSec

CloudHub
use the VPG to route between customers

**Software VPN**
eg OpenVPN
run on EC2

**Transit VPC**
a hub where everything meets
allows for other cloud providers

**VPC to VPC**
everything above applies
+ VPC Peering


**VPC Peering**
uses the AWS backbone
not transitive

process - 
server 1 sends a peering request
server 2 accepts the peering request
add routes to both servers

Cross Region peering is allowed

**Private Link**
exposing VPC endpoints over the aws backbone - S3 

Interface Endpoint vs Gateway Enddpoint

Interface Endpoint
- ENI with a private IP
- dns to direct traffic
- API Gateway Cloud Formation ...
- security groups for securing


Gateway Endpoint
- S3 and DynamoDB
- gateway that is a target for a specific route
- prefixes in the route table
- secured using VPC Endpoint Policies

## Internet Gateway
- redundant
- highly avaliable
- no risk or bandwidth

IPv4 andIPv6 support

what does it doe?
1. route target for internet bound traffic
2. NAT for public IPs (not for instances that only has private IP)

Egress Only Gateway
- only outbound is allowed

**NAT Instance**
- ec2 instance with a special AMI
- translate many private IP to a single public IP

does not support IPv6
use egress only

add default route to the private subnet

**NAT Gateway**
fully managed
create multiple NAT gateways in multiple AZs
Cant use a NAT Gateway to do VPC Peering, VPN or Direct Connect

NAT Gateway vs NAT Instance

|Gateway|Instance
-|-
Availability|Highly avaliable within AZ|on your own
Bandwidth|up to 45 Gbps|depends on bandwidth of instance type
maintenance|managed by aws|you
performance|optimized for NAT|AWS AMI
public IP|elastic IPcannot be detached|eip can be detached
security group|cannot be associated with NAT gatway|can use security group
bastion Server|no|yes

## Routing
- every VPC has a implicit route table
- local router for the cidr block
- most specific route wins

Broadcast address not supported

**Border Gateway Protocol**
- allows dynamic routes
- required for direct connect
- optional for VPN
- BGP Community tagging
- TCP 179 + ephemeral ports
- Autonomous System Numbers (ASN) unique endpoint identifier
- higher weights are prefered

can weight your connections

## Advance Networking

**Additional Drivers**
Intel VF Interface - 10Gbps
Elastic Network Adapter - 25 Gbps

**Placement Groups**
put resources close to one another
being intentional about placing our instances

clustered vs spread

clustered - place in a low lat group in a single az. low latency and high through put
finite capacity - launch all upfront

spread - HA, placed on different hardware
7 instances in a group per AZ

A spread placement group can span multiple Availability Zones in the same Region. You can have a maximum of seven running instances per Availability Zone per group.

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html

## Route 53

Policies

1. Simple 
2. Failover
3. Geolocation (by continent)
4. Geoproximity (by distance)
5. Latency
6. Multivalue Answer
7. Weighted

## CloudFront
- CDN
- static to 4k live streaming
- edge location concept
- certificate manager
- supports SNI

custom ssl cert 
- wildcards
- host key verification

Option 1: private EIP on each cloudfront edge
Option 2: SNI checks host headers

can choose the SSL/TLS version

## Elastic Load Balancer (ELB)
layer 7 load balancer

Application LB vs Network LB vs Classic LB

Application LB
path based load balancing

Network Load Balancer
layer 4
based off protocol
fast io

**Sticky Sessions**
LB remembers which server it routed to.

## Additional Resources

https://d0.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf

https://d1.awsstatic.com/whitepapers/Networking/integrating-aws-with-multiprotocol-label-switching.pdf

https://d1.awsstatic.com/whitepapers/Security/Networking_Security_Whitepaper.pdf

https://www.youtube.com/watch?v=KGKrVO9xlqI

https://www.youtube.com/watch?v=8gc2DgBqo9U

https://www.youtube.com/watch?v=z0FBGIT1Ub4

# Security

Shared Responsibility Model
- client has alot of responsibility

Principle of Least Privilege

Concepts
1. Identity
2. Authentication
3. Authorization
4. Trust

Identity Store
Identity Providers
Service Providers

Federated Identity Protocols
SAML vs OAuth vs OpenID

SAML - original protocol. xml based. user group and member information for enterprises.

OAuth - handles authentication.

OpenID Connect - built on top of OAuth. SSO for public customers

https://www.gluu.org/resources/documents/articles/oauth-vs-saml-vs-openid-connect/

**Multi Account Management**

AWS Tools for account management
1. AWS Organizations
2. Service Control Policy (SCP)
3. Tagginghttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdf
4. Resourchttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdfps
5. Consolihttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdfBilling

Account Sthttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdfe
1. Publishhttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdfcount
2. Identithttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdfunt
3. Logginghttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdfnt Account
4. Informahttps://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdfecurity Account
5. Central IT Account Structure
   
Consolidated Billing
Consolidated Security

SCP - Cascade Down -> affects all sub accounts

**Network Controls and Security Groups**
Security Groups - firewalls

NACLS
- subnet level

SG vs NACL
https://medium.com/awesome-aws/aws-difference-between-security-groups-and-network-acls-adc632ea29ae


**Directory Service**

Cognito - offload sign up and sign ins
AWS Cloud Directory - cloud native directory
AWS MS AD - fully managed ms AD running enterprise version
AD Connector - connect local ad
Simple AD - Samba impl. simple user directory plus ldap compatibility

AD Connector vs Simple AD


AD Connector
- must have existing on premise AD
- Existing AD users can access AWS assets via IAM roles
- Supports MFA via existing RADIUS based MFA infrastructure

Simple AD
- Stand alone AD based on Samba
- supports user account and groups, group policies and domains
- keboeros based SSO
- MFA not supported 
- No trust relationships


**Credential and Access Management**
json used in policy definitions

**STS**
temp credential access to users
https://web-identity-federation-playground.s3.amazonaws.com/index.html

**Token Vending Machine**

Anonymous TVM
Identity TVM 

**AWS Secrets Manager**
- to store passwords or keys
- automatically rotates our RDS passwords
- non encrypted

**Encryption**
At rest vs in transit

**AWS Key Management Service**
tight integration
encrypted key manager

**CloudHSM**
- dedicated hardware device
- single teneted
- dosent integrate nativily
- custom write your applications 
- issue CA
- Customer manages the root of trust

AWS Certificate Manager
- sign ssl/tls certificates
- supports wildcard domain
- does renewals automatically
- can create private certificates

**DDoS**
- PLP - lock down nacls and sgs
- cloudfront!!
- static content on s3
- aws sheild
- aws firewall
- baseline triggers alarms
- route 53 based on geography

**Intruder Detection System**
- less invasive
- fail open
- detect not block
- SIEM

**Intruder Prevention System**
- can block traffic
- blacklist
- Log collection cloudwatch or S3

Security Information and Event Management System (SIEM)

**Bastion Server**
- single entry point in the DMZ
- only have to secure this

CloudWatch vs CloudTrail

CW - operations - alarms
Ct - activities - API

**Service Catalog**
- prevent messes
- framework for admins to create pre defined packages
- granular control
- makes used of adopted IAM roles

Constrains 
1. Launch Constraint - allow end user to deploy without having the proper perms
2. Notification Constrain - sns topic
3. Template Constraint - wizard like
4. Shared Portfolio - cascades down
5. by default - the launch role is inherited from portfolio


Additional Resources

https://d1.awsstatic.com/whitepapers/aws-security-best-practices.pdf

https://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdf

https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf

https://aws.amazon.com/whitepapers/#security

https://www.youtube.com/watch?v=gjrcoK8T3To

https://www.youtube.com/watch?v=aISWoPf_XNE

https://www.youtube.com/watch?v=tzJmE_Jlas0

https://www.youtube.com/watch?v=71fD8Oenwxc

# Migrations
Best practices for taking on premise servers to the cloud


Strategirs
1. Rehost



## Strat 1: Re Host
just move all on premise servers to the cloud
eg move mysql instance to mysql instance running on the cloud

## Strat 2: Re Platform
similar to s1
but leverage RDS instead

## Strat 3: Re Purchase
Drop and Shop
eg on premise crm to salesforce

## Strat 4: Rearchitect
redseign in a cloud native manner
traditional backend to lambda

## Strat 5: Retire
get ride of lagacy applications

## Strat 6: Do Nothing
does what it says

**TOGAF**
The open group architectural framework
defacto standard of enterprise architecture

Cloud Adoption Framework
1. Business
2. People
3. Governance
4. Platform
5. Security
6. Operations

## Hybrid Architectures
- Both on premise and cloud resources
- first step in a migration
- VMWare


eg
- S3 with storage gateway
- SQS with middleware
- VMWare vCenter plugin


## Migration Tools

1. Snowball Service
2. Storage Gateway
3. Server Migration Service
4. Data Migration Service
5. Schema Conversion Tool
6. Application Discovery Service


## Server Migration Servic
- automates migration of VMsto AWS
- replicatesVMs to AWS syncing volumesand creating perodic AMIs
- Linux and Windows VMs

## Schema conversion tool (SCT)
can convert schemas to other databases
used for more complex usecases
eg data warehouses

## Database Migration Service
smaller and simpler conversions
supports mongo and dynamoDB
replication function for on prem to AWS

## Application Discover Service
- discovers all the services running
- can help in predicting cost for running the same setup in the cloud
- runs agentless in an vmware env via discovery connector
- or agent based in a non vmware env
- supports only linux and windows

**Migration Hub**
consolidated tooling


## Network Migrations

cutovers
- check for no overlaps in IP addressing
-  5 reserved IPs
- /16 to /28 cidr block
- no broadcast address

VPN is good first step
then Direct Connect with VPN as a redundency

VPN to DC is easy
- enable BGP routing or static route
- and weight DC higher
- AWS always prefers DC over VPN route

## Snow Family
- Evolution of the AWS Import/Export process
- as fast as youre willing to pay
- encrypted at rest and transit


Option 1: **AWS Import/Export**
- ship a harddisk to aws and they load it to S3

Option 2: **Snowball**
- aws ships it to you 
- up to 80TB
- send snowball back to aws
- aws copies data over to S3

Option 3: **Snowball Edge**
- like a snowball
- but with compute power like lambda and clustering
- portable AWS
- process data in a really remote location

Option 3: Snowmobile
- container truck
- up to 100PB of data


Tech is a minor aspect when planning a migration to the cloud

## Additional Resources

https://d1.awsstatic.com/whitepapers/Migration/aws-migration-whitepaper.pdf

https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf

https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf

https://d1.awsstatic.com/whitepapers/AWS-Cloud-Transformation-Maturity-Model.pdf

https://www.youtube.com/watch?v=Y33TviLMBFY

https://www.youtube.com/watch?v=9wgaV70FeaM

# Architecting to Scale

Loosely Coupled Architecture Benefits
- layers of abstraction

Horizontal vs Vertical Scaling

Scale in vs Scaling down

4 Scaling Options
1. Maintain
2. Manual
3. Schedule
4. Dynamic
   
**Launch Config**
- first step to auto scaling
- vpc and subnet
- can attach to ELB
- health check grace period -defines how long to get settled before its subject to a health check
- scale based on different factors


**Scaling policies**
1. Target Tracking Policy - eg cpu utilizations exceeds 70%
2. Simple Scaling Policy (cool down period)
3. Step Scaling Policy - more gradular controls (warm up period)

**Scaling Cooldown**
- come up to speed and absorb load
- scale down should hae a smaller cool down
- time taken for it to setup and stabilize

cooldown vs grace period
- grace period is time subjected for a health check
- cooldown is time taken before another scalling event can take place
  
## Kinesis
- Collection of services
- made up of shards
- each shard can inject 1000 records per second
- default shard limit is 500
- record is what comes in
- record contains partition key, sequence number and data blob
- transient data store
- write out to a database or S3 or process it

**Kinesis Consumer Library**
recommended way to console data from kinesis

**Kinesis Producer Library**
recommended way to push data into kinesis

**Video Streams**
**Firehose**
**Real Time Analytics in real time**

shards - lane in a highway


## DynamoDB
- 400KB limit per item
- unlimited item

**Partition Calculations**
By Capacity  (read per 3000 writes per 1000)
By Size
Total Partition
partitions = ceil(max(capacity, size))
rw capacity spread evenly


partitioning is based on hash

autoscaling for dynamoDB
- increase partitions
- uses target tracking
- no way to scale down if consumption drops to zero - 
  - send minimal request until it scales down
  - manually reduce the max capacity to be the same as the minimum capacity
  - also supports global secondary indexes

## CloudFront
- static and dynamic content
- support flash media RMTP protocol
- supports media and live streaming over http
- multiple origins S3 EC2 ELB or another webserver
- multiple origins can be configured
- Behaviours can be used to filtered based off url


Invalidation
- delete the file from the origin + wait for ttl to expire
- use CLI to send an invalidate request based on resource or path
- third party tools

**Zone Apex Support**
Allows based urls github.com

**Geo-Restriction**
can be used to allowed  only certain countries to view content.

## Simple Notification Service
- pub sub model
- topics are like an outbox
- subscriptions to topics 
- can be used with device messaging
- can be used to decouple applications

**Fanout Architecture**
- multiple listeners

## Simple Queue Service
- fully managed 
- transient storage
- FIFO support
- 4 days default lifetime
- 14 days max lifetime
- max message size of 256kb
- Java sdk to store in s3 and enqueue the pointer to sqs instead
- scaled by increaseing workers

**Standard Queue**
- no assurance to the order

**FIFO Queue**
- maintained the order in which the messages are rv
- if anything happens to a message itll hold up all the other messages behind it


**Amazon MQ**
- impl of Apache of active MQ
- message broker
- less tightly integrated
- fully managed and HA within a region
- websocket support
- MQTT JMS

## Lambdas
- serverless
- node py go c# java
- stateless
- event basis
- no limit to scaling
- aws handles the scaling

**Dead Letter Queue**
- throw all the errors to the DLQ
- and trigger a notification
  
## Simple Workflow Service
- static tracking system
- supports sync and async
- human enabled workflow
- step functions over simple sws
- long polling

Activity Worker - does the work

Decider - coordinates the tasks

## Step Functions 
- in the context of lambdas
- managed workflow and orchestration plateform

**Amazon State Language**
- json based

**State Machine**


## Batch
spins up scheduled batch jobs on ec2 instances

- manage batch processing on EC2 instances
- managed on unmanaged 
- spot or on demand 
- job queue
- job definition
- 
step over sws for new applications - aws recommends

Step functions vs Batch

use cases

Order processing flow - step
manual steps or intervention required eg load application process - SWS
Store and forward  eg Image Resize process - SQS
scheduled or recurring tasks with out heavy logic - Batch

## Elastic Map Reduce
- isnt a single product
- a collection of open source project
- hadoop, hbase, hivee


Hadoop HDFS and MapReduce 

HDFS - just the format that data is stored to make it easy to process

MapReuce - is the process

Zookeeper - resource coordination

Oozie - workflow framework

Pig - scripting framework

hive - sql interface

mahout - machine learning componenet

Hbase - columner database for storing data

Flume - log collection

Sqoop - import of data from other data sources

Ambari - Management and monitering tool

Enterprise support - HORTONWORKS and CLOUDERA

**Elasitc Map Reduce (EMR)** 
managed hadoop cluster
log analysis or financial analysis

Step - a programmtic task performed on the data eg count words

Cluster - collection of EC2 instances provisioned by EMR

**Setup**
Master Node - 
Core Nodes - hdfs
Task Node - ephemeral storage

Additional Resources

https://d1.awsstatic.com/whitepapers/aws-web-hosting-best-practices.pdf

https://d0.awsstatic.com/whitepapers/aws-scalable-gaming-patterns.pdf

https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

https://d1.awsstatic.com/whitepapers/cost-optimization-automating-elasticity.pdf

https://www.youtube.com/watch?v=w95murBkYmU

https://www.youtube.com/watch?v=dPdac4LL884

https://www.youtube.com/watch?v=9TwkMMogojY

https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf

https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf

# Business Continuity
and disaster recovery

> Everything fails all the time therefore we need to architect for everything to fail. - Werner Vogels

**Business Continuity**
seeks to minimize business activity disruption when something fails

**Disaster Recovery**
the act of responding and recovering from an event that threatens business continuity


Fault Tolerance vs High Availability
- similar
- FT is concern with not allowing users o know that a fault happened
- HA leaves some room for downtime

Service Level Agreement (SLA)
- can claim service credits if disrupted


**Recovery Time Objective (RTO)**
time it takes to get back to usual

**Recovery Point Objective (RPO)**
acceptable amount of data loss measured in time

**Business Continuity Plan**
- defines RTP and RPO
- defines HA investment

**Categories of Disasters**
1. Hardware Failure
2. Deployment Failure
3. Load Induced
4. Data Induced
5. Credential Expiration
6. Dependency
7. Infrastructure
8. Identifier Exhaustion
9. Human Error

**DR Architectures**
1. Basic Backup and Restore - offsite backup
2. Pilot Light - minimal standby instances on aws. manual intervention
3. Warm Standby - standby already running.
4. Multi Site - auto fail over
   
## Storage Volumes
### EBS
- 0.2% failure rate
- single az replication
- snapshot - multiAZ

**RAID**
RAID0 - no redundency / aka striping / good read and writes

RAID1 - mirroring / complete replication / duplicate / slower on read and writes

RAID5 - 3 drive min, one can fail. capcity is n-1 drives

RAID 6 - 4 drives min 2 can fail. n -2 capacity. writes suffer.

AWS dosent recomend RADI 5 and 6 due to EBS high IOPS for house keeping

RAID0 - striping can impove throughput

### S3
- eleven 9s of durability
- standard - multizone avability
- single zone has sinle az durability

### EFS
implementation of NFS
multi AZ redundency

### Storage Gateway 
for offsite backup

## Compute Options
- ALWAYS KEEP AMIs UP TO DATE
- prefer horizontal scaling
- route53 to do health checks and failover


## Database considerations for HA
- DynamoDB has built in HA
- prefer Dynamo db for HA
- if need RDS use Aurora
- If cannot Auror then multi AZ RDS
- use frequent snapshots from the standby or read replication
- if running on EC2 youre on your own
  

### RedShift
- no multi AZ deployments
- best to just use multi node cluster and use replication

### ElasticCache

Memcached
- no replication
- use multiple nodes in each shard
- launch multiple nodes across availiable AZs

Redis
- use multi shards per node
- multi AZ replications
- backups of redis cluster

## Networking
- subnets in multiple AZs
- redundant connection in VPG
- DirectConnect is not HA by default
- Route53 can do failover
- use elastic IP without dealing with dns
- spin up a nat gateway in each AZ with routes for private subnets to use the local gateway

**Additional Resources**
https://d1.awsstatic.com/whitepapers/Storage/Backup_and_Recovery_Approaches_Using_AWS.pdf

https://d1.awsstatic.com/whitepapers/getting-started-with-amazon-aurora.pdf

https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf

https://www.youtube.com/watch?v=xc_PZ5OPXcc

https://www.youtube.com/watch?v=RMrfzR4zyM4

https://www.youtube.com/watch?v=a7EMou07hRc

# Deployment Options

Types of Deployments
1. Big Bang
2. Phased Rollout
3. Parallel Adoption

Rolling Deployment
- new launch config with a new ami
- terminate old ec2 instances

A/B Testing
- using weighted round robin on Route53

Canary Release
- deploy a version two in one instance 


Blue Green Deployment
- play with route53 using route53
- easy rollback option
- or use ELB 
- or use elastic beanstalk to point to new url
- OpsWorks to clone the stack and update DNS

When not to use blud green (Contraindication)
- application is tightly coupled to the data schema
- special upgrade routine or post processing

## CICD

Continuous Integration
- merging back into master frequently testing as we go

Continuous Delivery
- one button deployment

Continuous Deployment
- automatically deploys the entire chain

Consideration
- smaller incremental improvements and features
- strong test automation game
- feature toggle
- gels well with micro services

**CodeCommit**
hosted git repo

 **CodeBuild**
 compile and run test build deployment packages

 **CodeDeploy**
 can deploy deployment packages

 **CodePipeline**
 orchestration tool

 **XRay**
 used for debugging in s serverless and distrubted system

 **CodeStar**
 leverages all the other services and automates

 **Elastic BeanStalk**
- makes deployment dead simple
- deploy docker to PHP and Java
- supports multiple env
- application versioning supported

## CLoudFormation
- IaaC
- Infrastructure as Code
- templates
- stacks - actual resources
- change sets - summary of changes to a template

**Template**
- json or yarm
- only resources section is required
- stack policies (protect resources)
- cannot remove stack policy
- by default denys all

**Best Practices **
- python helper scripts avaliable for ec2 instances
- use cf to make changes to your stack
- make use of changesets to detect errors
- use stack policies to protect resources
- use a vcs to record changes

## Elastic Container Service
ECS and EKS

**ECS**
- build by amazon
- simpler
- leverage aws integrations
- containers are called Tasks
- isolated
- limited extensibility

**EKS**
- pure k8s
- feature rich
- containers called pods
- shared access to each other
- k8s addons

ECS Launch Types
1. EC2 Launch Type
2. FarGate

EC2 Launch Type
- explicitly provision EC2 instances
- responsible for maintaining the pool
- responsible for cluster optimization
- more granular control

Amazon Fargate
- automatically provision resources
- handles cluster optimization
- limited control

## API Gateway
- edge optimized
- supports private availability
- SNI is supported
- does caching

## Management Tools

### AWS Config
- audit and access configuration
- helps with itil
- creates a baseline
- and tracks changes over time
- can set rules to check for complience
- eg is backup enabled on RDS?

### OpsWorks
integrate with chef and puppet
from upgrades to deployment

1. Chef Automate
2. Puppet Enterprise
3. OpsWorks Stacks (compatible with chef recipies) - can be cloned but only within the same region

### Systems Manager
centralized console and toolset for a wide variety of system management tasks

- SSM Agent 
- installed on morden AMI

1. Inventory Service
   1. determine which instances have old versions of software
2. State Manager
   1. creates and manage states
3. Logging
   1. aws recomends using CloudWatch instead
4. Parammeter Store
   1. integration with KMS
   2. eg rds credentials
5. Insights Dashboard
   1. account level view of CT config and TA
6. Resource Groups
   1. another way to get to the resouce groups page
   2. organize through tagging
7. Maintenance Window
   1. define matainince windows
8. Automtation
   1. run scripts
   2. run commands without ssh/rds
   3. on multiple machines concurrently
9. Patch Manager
   1.  automated patch
   2.  apply during a matainence windows
   3.  baseline for patches
   4.  aws have predefined
   5.  7 day timer

**Documents**
- ssm documents
- json or yaml
- steps and params
- stored as version

1. Command Document
2. Policy Document
3. Automation Document


## Additional Resources

https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf

https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf

https://d1.awsstatic.com/whitepapers/overview-of-deployment-options-on-aws.pdf

https://www.youtube.com/watch?v=01hy48R9Kr8

https://www.youtube.com/watch?v=Qik9LBktjgs

https://www.youtube.com/watch?v=GEPJ7Lo346A


# Cost Management

Types of expense
1. Capital Expense
2. Operation Expense

One of the benifits of cloud computing is Operation expense


**Total Cost of Ownership**
- A comprehensive look at the entire cost model of a given decision

**Return on Investment**
- amount and org can expect to recieve back
- may not always be positive

be skeptical of TCO and ROI

Cost Optimization Strategies

Strategy 1: Appropriate Provisioning
- turn off not needed instances
- consolidate where possible
- eg one big dynamo db database
- cloudwatch can help with this
- scale in if unused

Strategy 2: Right Sizing
- use the lowest cost resource that meets the specs
- use sqs to decouple

Strat 3: Purchase Options
- used reserved instaces for permanent
- spot instances are best for temp horizontal scaling
- EC2 Fleet target mix of on demand reserved and spot instances

Strat 4: Geographic Selection
pricing can vary from region to region
sa-east-1 is rlly ex

Strat 5: Managed Services
use managed services whenever possible
rds over mysql on ec2 
RDS Redshirt fargate EMR
lower complexity and cost

Strat 6: Optimized Data Transfer
- transfer in is free
- transfer out has costs

use direct connect might be more cost effective in the long run

## Tagging
- the number one best thing to manage aws assets
- metadata
- arbitary key value names
- can be used for cost allocation security automation and many more
- can enfore tagging standaization with AWS config

## Resoure Groups
- makes use of tags
- to group resources together

**Reserved Instances**
- only garduring DR
- significant discount
- automatic when launching instances of the same type
- can be shared across multiple accounts with consolidated billing

types
1. Standard
2. Convertible (can change instance family, OS, tenancy, payment option)
3. Scheduled

can be sold on a secondary marketplace

40-60% discount

RI Attributes
- Instance ype
- Platform
- Tenancy
- Availability Zone (only apply to the az)

Other resources in the same family.
instace size flexibilty only available for linux regional RI
m4.2xlarge = m4.xlarge times 2

**Spot Instances**
1. create a spot request
2. specify AMI, VPC , instance type
3. define a highest price

types 
- one off
- maintain (can set to hibernate)
- duration-based (based on run time)

Consideration
- cost depends on az and instance type
- look at the pricing history of instace and az

**Dedicate Instance**
- shared within your instances
- a little more expensive
- still a vm

**Dedicated Host**
- dedicated hardware
- - server bound software 
- dedicated host reservation
- if you need sockets 
- can only run one type of ec2 instance
- but resides on hardware you dont share with anyone


## Cost Management Tools

### AWS Budgets
- set pre defined limits

## Consolidated Biling
- one account to pay for many accounts
- eg S3 tiered pricing
- achieve some economy of scale

## Trusted Advisor
- can check if we are wasting money
- core checks (for anyone)
- more extensive testing avaliable for enterprise plans

## Additional Resources

https://d1.awsstatic.com/whitepapers/architecture/AWS-Cost-Optimization-Pillar.pdf

https://d1.awsstatic.com/whitepapers/total-cost-of-operation-benefits-using-aws.pdf

https://d1.awsstatic.com/whitepapers/introduction-to-aws-cloud-economics-final.pdf

https://www.youtube.com/watch?v=CcspJkc7zqg

https://www.youtube.com/watch?v=XQFweGjK_-o

https://www.youtube.com/watch?v=1Z4BfRj2FiU
