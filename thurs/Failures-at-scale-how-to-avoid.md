# Failures at scale and how to avoid them

7 or 8 things to do to keep you up pretty much all the time

"Every day AWS adds enough new server capacity to support all of
Amazones global infrstrcuture when it was a $5.2B annual revenue
enterprise (in 2003)"

What seems to be wrong:

Multi DC on two coasts - you cannot do it sync, so you try async, but
 1-2ms to SSD, 74ms from NY -> LA. If you fail over, you lose
transactions. It's no win and very hard to get right. Or you don't fail
over and you lose availability.

Active/Passive is fragile, since you never fail over it's not tested
much.

Two way redundancy is expensive. Need more than 1/2 capacity to handle
failure, so really you want three way redunacy, but so much harder and
impracticle.

How amazon does it:

* choose correct region
* can replicate between az
  - can commit async cause can be 2-3ms
* stateless ec2 apps

ROC design pattern (Recovery oriented computing):

* bohr bug
  - should be found pre-prod, functional bug
* heisenbug
  - software issue that only occurs in unusual times (e.g. cross request
    timing issues, load at a certain level, plus some latency etc)
  - restart -> reboot -> re-image -> replace
  - don't let the automation do more than a junior engineer can do

Canary in the DC
  - route increared load to a single server
    - lets you see load levels for a server
    - when you see problems, delays etc
    - can then reduce the load to find sweet spot


* greaceful degredation
  - first shed non-critical workload
  - then degraded operations mode
    - e.g in search, only show answers from the first 5M results, as
      opposed to whole search
    - e.g. email, don't deliver things tagged as spam
  - finally admission control
    - don't let more customers in
  - related idea - metered rate-of-service admission
    - allow in small increments of users when restarting after failure
* EC2 best practise, do not aquire new resources when failing away


Continuously test in prod
* active/active and test in prod
* game days
* chaos monkey
  - pointed at a set or resouces
  * certain time frame for instance kill
  * app config optoins (including opt out)

Replaced 2-way redundent network stuff with N-way
  - good when doing updates etc, as multiple machines
  - good for costs, less big routers, more small routers
  - monitor to poinpoint faulty routers, or link fault before app
    impacct


http://mvdirona.com/jrh/talksAndPapers/JamesRH_Lisa.pdf
http://mvdirona.com/jrh/work/
