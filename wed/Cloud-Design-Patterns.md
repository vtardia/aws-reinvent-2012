# Cloud Design Patterns

Ways of descriping cloud things without using all the AWS terminology:

Floating IP - moving an EIP to new instance

Weighted Transiition - use weighted rr dns with route53 to switch to a
new ELB (and new stack behind it) - or just allow a certin % of traffic
to go there

Job Observer - one group of ec2 put into sqs, other group ec2 pull from
queue process - cloudwatch watches queues, alarm can then hit autoscale
to scale the process groups

[Wiki](http://aws.clouddesignpattern.org) with lots of information and patterns, need to translate - also on en.

