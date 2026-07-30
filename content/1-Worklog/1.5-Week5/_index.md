---
title: "Week 5 - DynamoDB Fundamentals & Global Infrastructure CloudFront"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
url: "/en/1-worklog/1.5-week5/"
---

### Weekly Topic

Non-relational Database + Global Infrastructure Content Delivery Network

### Weekly Objectives

* Understand and operate a high-performance non-relational database service.
* Leverage global infrastructure to reduce web content access latency.

### Work Schedule

| Date | Day | Task Description | Reference Links |
| :--- | :--- | :--- | :--- |
| 29/06/2026 | Monday | - Grasp DynamoDB database model (NoSQL)<br>- Learn about Partition Keys and Sort Keys<br>- Read documentation on compute capacity (RCU/WCU) | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ |
| 30/06/2026 | Tuesday | - Practice DynamoDB operations:<br>&emsp;+ Initialize a user data table<br>&emsp;+ Use UI to insert (PutItem) and fetch (GetItem) data<br>&emsp;+ Differentiate Scan and Query operations | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html |
| 01/07/2026 | Wednesday | - Explore Amazon Route 53 DNS service<br>- Understand CloudFront CDN architecture<br>- The role of Edge Locations | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/ |
| 02/07/2026 | Thursday | - Configure Content Delivery Network:<br>&emsp;+ Initialize new CloudFront Distribution<br>&emsp;+ Point Origin to the static S3 Bucket<br>&emsp;+ Configure Cache Behavior policies | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/GettingStarted.html |
| 03/07/2026 | Friday | - Assess access performance via CloudFront links versus raw S3 links<br>- Discard drafted Route 53 configurations<br>- Summarize week 5 progress | https://cloudjourney.awsstudygroup.com/ |

### Expected Outcomes

* DynamoDB tables are properly designed for high performance with rational partition keys.
* Static content is cached at edge locations of the global infrastructure.

### Week 5 References

* Amazon DynamoDB Docs: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/
* Amazon CloudFront Docs: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/
