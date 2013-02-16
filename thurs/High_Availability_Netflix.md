**Links**: [Video](https://www.youtube.com/watch?v=dekV3Oq7pH8&list=PLhr1KZpdzukee0jM3DusklQBs1pPntvDu&index=3) | [Slides](http://www.slideshare.net/AmazonWebServices/arc203-netflixha)


Heavily SOA oriented, no single service failure can take the service down.

Their entire environment is duplicated across availability zones using Auto Scaling Groups

## Chaos Gorilla 

Deployed service that can randomly kill a whole zone

OR

Can programmatically move a whole zone to a new AZ

## Triple replicated persistence

Use Cassandra to replicate all data to three zones

To upgrade Cassandra, they take the node down, upgrade and then rejoin. All data replicated to new node.

## Isolated regions

With EU launch, wanted a replica in the EU. Also has different demands to needs bespoke AutoScaling schedule

Asynchronous replication between regions

## Failure

Short timeouts - if something isn't answering quickly it's probably broken. Fail fast and alert.

## Taxonomy of Failures

### Zone Failures Modes
- zone power outage. clean, fast fail, easy to recover from
- zone network outage. slow fail, requests backup, often flip flops
- zone dependent service outage. EBS, SQS, etc might fail. hard to debug

### Regional Failure Modes
- regional network outage. DNS, router death, etc
- control plane overload. typically caused by local failures rippling out (e.g. cascading cassandra problem)

### Global Failure Modes
- software bugs
- global configuration changes
- cascading capacity overload. rerouting traffic from dead AZ/region can cause cascading problems
