# Provisions AWS stuff using Ansible

Ansible playbooks to provision taxonomy tagger ecosystem in AWS
See [aws-infra.png](https://github.com/cmcornejocrespo/ec2-provisioning-sample/blob/master/aws-infra.png) for more details.

## Requirements
- Ansible must be installed
- AWS creds must be setup

This project is meant to be run by a Jenkins instance.

## Dev

For dev environment we don't need to provision the same infra as is just for test.
For the moment we use Frankfurt as region for dev!

- aws_policy
- aws_ec2_group
- vm for for monitoring
- vm for splunk
- vm for rest-tagger
- vm for job-tagger
- aws_sqs for efc-jobs
- aws_sqs for bg-jobs

## Sandbox

For sandbox we are mimicking the infrastructure that we use for prod.
Until Devops move the ansible configuration to puppet, we'll use sandbox a prod.

Currently all the VMs are in one VPC on one subnet. We have:
- public load balancer
- private load balancer
- Tagger REST VMs (2x)
- Job Tagger VMs (3x efc-jobs, 3x bg-jobs)
- Redis (1x)
- SQS (1x efc jobs, 1x bg-feeds)
- Route53 for private load balancer

Security groups (inbound rules):
- job tagger - port 22
- public ELB - port 80 (specific IP addresses)
- private ELB - port 80 - only from within VPC
- tagger REST - port 22, 80 - only from public and private ELB
- elasticache - port 6379 - only from tagger REST

VMs use restricted IAM roles (for SQS, Elasticache and logs), there are no passwords in use.
