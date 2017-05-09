# stack-story
various stories about stacks

Before running, it needs proper account and access credentials.

## VPCs
Directory vpcs Template for creating a simple VPC with 2 public subnets.

AWS CLI:
```
aws cloudformation create-stack --stack-name dev-stack --template-body file://./vpcs/cf-inf.json --parameters file://./vpcs/cf-inf-params.json
```
Check the stack status until if completes. 
The resouces it will creates:
  - VPC
  - 2 public subnets
  - IGW 
  - Routetable
## asgs
This is the cloudformation for deploying a simple hello-world app. At the end of the deployment, it will deploy 2 docker containers (carinamarina/hello-world-web and carinamarina/hello-world-app).

AWS CLI:
```
aws cloudformation create-stack --stack-name app-stack --template-body file://./asgs/cf-app.json --parameters file://./vpcs/cf-app-params.json
```

Check the stack status unitl if completes.
The resouces it will creates:
  - ALB
  - AutoScalingGroup
  - 2 SecurityGroups for ALB and ASG each
  - LaunchConfig for ASG
  - InstanceProfile for target group health check
  - TargetGroup for ALB
  - ScaleUp and ScaleDown plus corresponding Alarm

## Template Thoughts
- Add rolling update policy for ASG for zero downtime deployment
- There is health check in launchconfig to make sure the instance is in health status before moving forward
- 2 templates are used for separation of infrastructure and application stacks.  Normally changes to infrastructure is less frequent.
- There are more can be added such as
  - Cloudwatch logs for ALB and Instance logs, 22 port can be get rid of if all logs (bootstrap) go to cloudwatch logs
  - Notification can be added for alarming
  - Nested Stack can also be used for modularizing template

## Questions
- The docker provided is a simple hello-world which is not a web app. So I chose another simple one for POC purpose.
