# Top 10 IAM Best Practices

1. Create individual users
2. Manage permissions with groups
  - map business functions to groups
3. Permissions - grant least privs
4. Configure a strong password policy
5. Enable MFA for privileged users
  - should assign it to provileged users
  - can have virtual or hardware devices
    - can use the google authenticator
6. Use IAM roles for EC2 instances
  - benefits
    - makes it easy to manage access keys on ec2 instances
    - automatic key rotation
    - assign least priv to the app
    - aws sdks are fully integrated
  - to use
    - create a role and give access
    - launch ec2 instances and add roles to them
    - that ec2 instance can then use the services defined in the role
    - on the instance you can then hit the metadata and get the roel
      info, it will give you the key, secret and expiration time for
      them, sdk handles this, you need to handle manually
7. SHarint - Use IAM roles to share access
  - this is new
  - no need to share credentials
  - cross account access
    - create a rolt
    - specify who you truest
    - descibe what the role can do
    - then share the nname of the role
  - inter-account delegation
8. Rotation - Rotate security credentials regularly
  - policy for password changing:
    - res: arn:aws:iam::AC:user/uname = action: aim:ChangePassword - effect: allow
  - policy for access key changing
    - need to allow: create delete, list, update, again on the same resource
  - access keys steps
    - create before delete
    - update to new access keys
    - deactivate old access keys
    - test for x amount of time
    - del old keys
9. Conditions - Restrict privs access further
  - can be enabled for any api
  lets you set the time of day people can come in on, ip etc
10. Remove or reduce the aws root account
  - don't use it


Question I asked:

Q: I'd like to have the ability to create IAM users who can launch ec2
instances with specific tags, then only terminate instnces with those
tags (e.g. tagged by application). Can this be done?

A:  Not yet, speak to the EC2 guys. Common request. Closest thing would
be to create seperate accounts, and enable the cross account IAM
stuff to let them terminate/create instances in those accounts. But it
isn't ideal.


