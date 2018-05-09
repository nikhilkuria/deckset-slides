# Visual Content!

![](guzman-barquin-599211-unsplash.jpg)

---

## What do we do?

* All the images in trivago
* To manage and serve
* image-service

^ Suite of services to import and manage images

^ images are stored in S3, but the metadata stored in trivagoDB

^ resize, duplicate check

^ image-api - used by non hotel search apps, tHM

---

## AWS migration

^ content moving to AWS

^ last 6 months

^ gain more freedom

^ leverage available and managed services by AWS

![](dominik-schroder-14534-unsplash.jpg)

---

# What are we moving to AWS?

* image records in trivago db
* all the services hosted in DUS
* images are already in S3

^ meta records (1.5 B)

^ image-api

^ services, cron

^ refactor a bit

---

# The landscape now!

![inline](aws-now.jpg)

^ two types of consumers

^ those who read and write (tHM)

^ those who read (hotel search)

---

# The plan

![inline](aws-plan.jpg)

^ break connection to trivagoDB

^ All data to hotel search via streams

^ Data and services in AWS

---

# Long run

![inline](aws-rename.jpg)

^ Cut support from trivago DB

^ Data is 100% in cloud

^ Most services in AWS

---

# Milestones

* simple database replication
* creation of source of truth database
* create Kafka streams from AWS
* _migration of API reads and writes_
* _migration of other processes_

^ Almost nil experience

^ Experience from other teams

^ Design of milestones.

^ Learn and implement

---

# Milestone #1 : simple database replication
![](alternate-skate-154657-unsplash.jpg)

^ No data Transformation

^ Build some pipelines which can be improved later

---

# Moving the data over

![inline](m1-1.jpg)

^ Two major tables

^ image_data

^ partner_image

^ Direct replication

^ unknowns

---

# unknowns

* How do we get the data from DUS
* How do we make it reactive
* How do we store the data in AWS

---

# and ..

* _How do we get the data from DUS_ - kafka streams
* _How do we make it reactive_ - Kafka to Kinesis
* _How do we store the data in AWS_ - Aurora RDS

^ Most tables are already streamed out

^ sps toolkit (platform) used to sink

^ Store, Aurora RDS vs Dynamo DB

^ Example of how one milestone builds knowledge for the next

---

# and ....

![inline](vc-aws-m1-2.jpg)

---

# Milestone #2 : Source of Truth
![](jason-blackeye-509323-unsplash.jpg)

^ resuse pipelines in milestone 1

^ Decided on RDS

---

# What were we trying to solve

* Better data structure
* Better classification
* New unique identifiers
* Single source for our data

^ All the things we wanted to do with trivagoDB

^ More logical classification of images

---

# Transformation

![inline](m2-sot-structure \(1\).jpg)

---

# Getting Data to AWS

* Kafka streams from trivagodb
* Sink Kafka to Kinesis streams
* SPS toolkit (platform)

^ Similar to milestone 1

---

# Not really trivial!

---

# Dependencies

* item
* item2partner (advertiser connection)

^ To classify an image between accommodation and poi

^ accommodation_images and destination_images

---

# The plan

![inline](VC-AWS-m2-1.jpg)

---

# The execution

![inline](Visual_Content_AWS_M2-accommodation.jpg)

^ Additional Data to be sinked

^ Service to generate new identifiers

^ table in database

---

# The execution

* Sink of item and item2partner
* New image id generation
* Delay in the sink of meta tables

---

# MILESTONE #3 : Data delivery
![](austin-neill-308610-unsplash.jpg)

^ Data is only good if someone can use it

---

# Publisher DB

* Synchronised version of Source of Truth
* Active images
* Images that can be used directly
* Kafka streams from db

---

# What did we need?

* Stream data from Source of Truth
* Filter data reactively
* Create Kafka streams

---

# What did we use?

* _Stream data from Source of Truth_ Kibin (RDS to Kinesis)
* _Filter data reactively_ lambda on Kinesis
* _Create Kafka streams_ Kabin (RDS to Kafka)

^ Grand example of sharing solutions within content

^ Custom built tools within content

---

# Kabin and Kibin

* Binary log readers
* _Kibin_ -> Descriptive
* _Kabin_ -> Identity

^ Ki for Kinesis

^ Ka for Kafka

---

# Running Kibin

* Initial Bootstrap
* _Data Feeder on ECS_
* Kibin as an EC2 instance

---

# How it looks

![inline](Visual-AWS-M3.jpg)

^ Single Kibin instance for all Tables.

^ Sieve lambdas

---

# Running Kabin

* Stream per table
* Kabin as an EC2 instance

---

# Full picture

![inline](Visual-AWS-M3- full.jpg)

^ Single Kabin instance for all Tables.

---

# Monitoring
![](chuttersnap-419183-unsplash.jpg)

---

# Cloudwatch Alarms

* Lambda errors
* Database load
* Kinesis Iterator Age
* EC2 errors

^ Kinesis Iterator Age!

---

# Notification Channels

* e-mail
* Slack

^ email was not very efficient

---

# Dashboards

* Cloudwatch
* _Lambdas_
* _Aurora Instances_

---

# Monitoring Business Metrics

* Looker
* Queries on Source of Truth
* Runs on Amazon ECS

^ Queries on Dynamo DB

^ ECS picks it up and stores results

^ Looker in progress

---

# Where are we now?
![](max-voxberg-392781-unsplash.jpg)

---

* Streams delivered from data in AWS
* _ACCOMMODATION MAIN IMAGES_
* All data stored in AWS
* New streams can be built from AWS

---

# Questions!

![](greg-rakozy-53292-unsplash.jpg)
