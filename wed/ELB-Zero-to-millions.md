# CPN205 - Zero to Millions of Requests


Can do internal load balancing - I think only for VPC:

elb -> instances in az's -> internal elb -> moar instances in az's


ELB is quite conservative now on scaling down, it used to be far more
aggressive.

20% increase traffic 5-10mins is OK - but big sudden bang (e.g.
100req/min -> 1Mreq/min in 30sec), it still can't deal with. Work
around by caching, or by drive new traffic to the elb to get it to ramp
up

## Testing

Remember to use distributed clients, and to ensure that you don't cache
DNS, as when things ramp up you'll get new IPs for the new ELB
instances. You want to be using these.

Bees with machine guns - they use it for testing

## NASA JPL - how they used ELB to help with video streaming

They wanted Tbit/s of traffic

Latency based routing with Route53

Cloudformation stack in `(())` could deploy each stack to different
AZ/Region:

`Video Stream -> Flash media server <- (( tier 1 nginx <-  tier 2 nginx (larger) <- elb)) <-  route 53`

Had to get the ELBs warmed up before hand.

"CloudTest" for benchmarking  - going for 25Gbit/s per stack - got
40Gbit/s out or one of them - this is hitting the T2 cache, the actual
origin server (the flash server) was only hitting 42Mbit/s

CloudFront behaviour to ensure that dynamic went to the elb, and static
to the other origins

## Q's for nasa:

Cost effective? Yes, from the point of view of not having to sign
contracts, or specify how much traffic they'd want. Only paied for what
they used.

Failed machines - users would get jitter, as they were basically doing
HTTP streaming, and the users client would re-connect to a new server
once it needed to refil its buffer.
