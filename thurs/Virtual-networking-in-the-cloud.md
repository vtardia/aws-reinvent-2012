# Virtual networking in the cloud

Virtual Networking with VPC

In EC2 we are all in one big 10. network. You then get a NAT'd public
IP.

VPC lets you launch and assign your own IP ranges and subnets for your
instances. Everything has its own private IP. Can now get an internet
gateway so instances can access the internet and S3 without having to
jump down a VPN via your DC.

Capabilities:
  - egress filtering
  - change SG membership on running instances
  - support all protocols
  - network ACLs
  - multiple elastic nics per instance
  - multiple ip address per instance
    - last two pretty much wouldn't come to ec2, other things in the
      future may be the same


Can have multiple SSL servers
  - multiple IP addresses on a single server for multiple ssl sites
Can do floating IP stuff, and do IP takeover
  - proper ha failover

The shell script they used to to this, basically pings the other
instance, if a ping fails then they called ec2-assign-private-ip, with
"allow-reassignment", that then moves the ip


You can do forensic subnets, so if an instance looks "bad" you can move it
into an isolated subnet that you can then analyse it.
  - you do this by attaching a new forensic interface to your instance,
    then remove the normal one


Multi-tiered security arch

ELB -> multi AZ web app firewalls -> internal ELB -> multi AZ app servers
  - seucirty groups at each stage let you force this traffic flow
  - can also setup route tables to then force the outbound traffic to go
    through services, such as IDS services
  - ips are limited, so that there are only public ips on the edge nodes

Using cloudformation you can automate the creation of VPC stacks in
different regions

