# Failover

This lab contains the following tasks:

## 5. Failover to the secondary region / simulate a regional failure and DR scenario

When used in combination with in-region replicas, an Aurora cluster gives you automatic failover capabilities within the region. With Aurora Global Database, you can perform a manual failover to the cluster in your secondary region, such that your database can survive in the unlikely scenario of an entire region becoming unavailable.

Combined with an application layer that is deployed cross-region (via immutable infrastructure or [copying your AMIs cross-region](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/CopyingAMIs.html)), you can further increase your applications' availability while maintaining data consistency.

#

>  **`Region 1 (Primary)`** 

1. First let us login to the Apache Superset interface
