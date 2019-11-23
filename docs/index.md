# ![AWS re:Invent 2019](../assets/reinvent19_banner.png)
# <img src="/assets/reinvent19_banner.png" alt="AWS re:Invent 2019">

# DAT348 - Amazon Aurora Global Database in Action

In this hands-on workshop, learn how to achieve multi-region resilience for your application backend by using Amazon Aurora global database. We focus on patterns for multi-region database rollout and real-world use cases. Get hands-on and learn how Aurora global database allows you to scale your infrastructure without having to implement complicated multi-region patterns and see how to best leverage Aurora global database for fast cross-region disaster recovery and low-latency global reads.

## Requirements
* AWS Account (Temporary Account provided on day of workshop)
* [Mozilla Firefox](https://www.mozilla.org/firefox/) or [Google Chrome](https://www.google.com/chrome/) web browser
* _PREFERRED:_ Familiarity with AWS [CLI](https://aws.amazon.com/cli), Console (EC2, RDS)

## Use the provided temporary AWS accounts

This workshop uses **AWS Event Engine**, a tool that dispenses individual workshop participants with a unique 12-digit alphanumeric code. This code should have been provided to you upon entry or beginning of the workshop, and it allows you to use a separate lab environment AWS account, without requiring the need for you to run this on your own personal or business accounts.

If you have not received one, please reach out to one of our support staff at the workshop.

Go to <a href="https://dashboard.eventengine.run/" target="_blank">**https://dashboard.eventengine.run/**</a> (open a new browser tab), enter the access code and click **Proceed**.

<span class="image">![EventEngine Login](ee-login.png?raw=true)</span>

On the **User Dashboard**, please click **AWS Console** to log into the AWS Management Console.

<span class="image">![EventEngine Dashboard](ee-dashboard.png?raw=true)</span>

Click **Open Console**. For the purposes of this workshop, you will not need to use command line and API access credentials.

<span class="image">![EventEngine Open Console](ee-open-console.png?raw=true)</span>

## Overview of labs (cleanup desc)

#### AWS Experience: Intermediate
#### Time to Complete: 75-100 minutes

We will run the following modules. Starting with the [setup](/).

# | Lab Module |  Overview
--- | --- | ---
1 | [**Setup**](/setup/index.md) | Set up the lab environment and provision the prerequisite resources
2 | [**Create Global Database**](/) | Create Global Database
3 | [**Connect Application**](/) | Connect BI apps
4 | [**Monitor Latency**](/) | Use CloudWatch to monitor for latency.
5 | [**Using Parameter Groups**](/) | Use different Parameter Groups
6 | [**Failover**](/) | Failover to the second region / simulate a regional failure and DR scenario
7 | [**Failback**](/) | Optional Challenge - failback to original region.
8 | **Decommission** | Not Required for Event Engine

## Lab environment at a glance (cleanup desc)

To ensure everyone has a consistent experience, we have created a foundational template for <a href="https://aws.amazon.com/cloudformation/" target="_blank">AWS CloudFormation</a> that provisions the resources needed for the lab environment.

<div class="architecture"><img src="/assets/images/generic-architecture.png"></div>

The environment deployed using CloudFormation includes several components:

*	a <a href="https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html" target="_blank">Amazon VPC</a> network configuration with public and private subnets
*	a <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html#USER_VPC.Subnets" target="_blank">Database subnet group</a> and relevant <a href="https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html" target="_blank">security groups</a> for the cluster and workstation
*	a <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html" target="_blank">Amazon EC2 instance</a> configured with the software components needed for the lab
*	<a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html" target="_blank">IAM roles</a> with access permissions for the workstation and cluster permissions for <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html" target="_blank">enhanced monitoring</a>, S3 access and logging
*	Custom cluster and DB instance <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html" target="_blank">parameter groups</a> for the Amazon Aurora cluster, enabling logging and performance schema
*	an <a href="https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html" target="_blank">Amazon Aurora</a> MySQL DB cluster with 2 nodes: a writer and read replica
* the master database credentials will be generated automatically and stored in an <A href="https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html" target="_blank">AWS Secrets Manager</a> secret.
*	a <a href="https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Integrating.AutoScaling.html" target="_blank">read replica auto scaling</a> configuration
*	a <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html" target="_blank">AWS Systems Manager</a> command document to execute a load test


## Other relevant re:Invent 2019 Workshops

Want more? If you wish to attend other related Aurora or globally distribued architecture workshops, please look into your event catalog schedule for the following:

* DAT327-R / DAT327-R1 - Accelerating application development with Amazon Aurora (Workshop)
* ARC317-R / ARC317-R1 - Building global applications that align to BC/DR objectives (Workshop)