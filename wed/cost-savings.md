## Rationale

3 regions and multiple zones per region
SOA. Around 20 teams who operate independently
Unconstrained deployment - freedom and responsibility

Maximise efficiency. Netflix try to always use reserved instances
Apply attention to costs

Asgard dashboard gives stats on available reserved instances to enable better cost management

## Methodology

- Manual
  - Weekly usage review, review reservation efficiency
  - Cost per key event - graph over time to measure overall efficiency
- Automated
  - Weekly email to teams with usage trends - engineers weren't checking the dashboards. Push notifications help spread info
  - Janitor monkey. Used to test efficiency
  - Cleans up resources that fall under certain usage thresholds

## AWS optimisations

- Align services to instance categories
  - 40% of instances at Netflix probably on a standard class - helps maximise reservation usage
- Autoscale for variable workloads
  - Without this you have to manually set instance counts. Often set arbitrarily high
  - Challenges
    - Service often need to scale on the same schedule
    - Use spare reservation capacity on back-end data processing, etc, during troughs in the schedule
  - Methodology
    - Start with bigger services
    - Identify trigger metrics (requests, load, etc). Most Netflix metrics are rate-based.
    - Scale up more agressively than scaling down
    - Push metrics to cloudwatch (using Servo). Cloudwatch can use your metrics to trigger autoscaling (up & down)
    - Autoscale batch apps (hadoop, etc) to use spare capacity
    - All manageable through Asgard
- Improve system utilization
  - Netflix aim for 45-60% CPU use for online processing
  - > 80% for batch processing

## S3

- Focus on reducing payload size and number of accesses
- Set up retention policies - set TTLs to automatically purge old data
- Monitor usage and look for spikes


Overall

- Reduce different AWS instance types
- Build pools of reserved instances
- Use batch processing to use capacity in usage troughs
- Don't use spot instances yet in favour of the reserved model
