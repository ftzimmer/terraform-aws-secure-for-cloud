# SingleAccount - Existing Cloudtrail

## Use-Case explanation

**Client Setup**

- [X] single-account setup
- [X] pre-existing resources
  - [X] cloudtrail
  - [ ] k8s cluster we want to use to deploy Sysdig for Cloud workload
  - [ ] ecs cluster/vpc/subnet we want to use to deploy Sysdig for Cloud workload


**Sysdig Secure For Cloud Features**

- [X] Threat Detection
  - [X] all accounts of the organization (management account included)
- [ ] Image Scanning (WIP?)
  - [ ] ECR pushed images
  - [ ] ECS running images
- [ ] CSPM/Compliance (WIP?)
- [ ] CIEM (WIP?)

## Suggested setup

For this use-case we're going to use the [`./examples/single-account-ecs`](../../examples/single-account-ecs/README.md) setup.
In order for this setup to work, all resources must be in the same AWS account and region.
Before proceeding, please read the example README and check whether you comply with requirements.

Please contact us if something requires to be adjusted.

### Step by Step Example Guide

Use `single-account` example with **`cloudtrail_sns_arn` parameter**

<!--
manual testing pre-requirements

0.1 Cloudtrail must exist. To be deployed on a separated terraform state

```
provider "aws" {
region = var.region
}

module "utils_cloudtrail" {
  source = "sysdiglabs/secure-for-cloud/aws//modules/infrastructure/cloudtrail"
  name   = "cloudtrail-test"
}
```

If cloudtrail is in another account
 {
      "Sid": "AllowCrossAccountSNSSubscription,
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::account-member:user/<SPECIFIC_USER>"
        # or
        #"AWS": "arn:aws:iam::account-member:root"
      },
      "Action": "sns:Subscribe",
      "Resource": "<CLOUDTRAIL_SNS_ARN>"
    }

-->


### Terraform Manifest Snippet

```terraform
terraform {
  required_providers {
    sysdig = {
      source  = "sysdiglabs/sysdig"
    }
  }
}

provider "sysdig" {
  sysdig_secure_url         = "<SYSDIG_SECURE_URL>"
  sysdig_secure_api_token   = "<SYSDIG_SECURE_API_TOKEN>"
}

provider "aws" {
  region = "<AWS_REGION>"
}

module "sysdig-s4c" {
  source = "sysdiglabs/secure-for-cloud/aws//examples/single-account-ecs"
  name   = "sysdig-s4c"

  cloudtrail_sns_arn  = "<CLOUDRAIL_SNS_TOPIC_ARN>"
}
```
