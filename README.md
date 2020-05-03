# terraform-aws-ecr

[![CircleCI](https://circleci.com/gh/tmknom/terraform-aws-ecr.svg?style=svg)](https://circleci.com/gh/tmknom/terraform-aws-ecr)
[![GitHub tag](https://img.shields.io/github/tag/tmknom/terraform-aws-ecr.svg)](https://registry.terraform.io/modules/tmknom/ecr/aws)
[![License](https://img.shields.io/github/license/tmknom/terraform-aws-ecr.svg)](https://opensource.org/licenses/Apache-2.0)

Terraform module which creates ECR resources on AWS.

## Description

Provision [ECR Repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Repositories.html),
[Repository Policy](https://docs.aws.amazon.com/AmazonECR/latest/userguide/RepositoryPolicies.html) and
[Lifecycle Policy](https://docs.aws.amazon.com/AmazonECR/latest/userguide/LifecyclePolicies.html).

This module provides recommended settings:

- Enable cross account access
- Automation of cleaning up unused images

## Usage

### Minimal

```hcl
module "ecr" {
  source          = "git::https://github.com/tmknom/terraform-aws-ecr.git?ref=tags/1.0.0"
  name            = "minimal"
  tag_prefix_list = ["release"]
}
```

### Complete

```hcl
module "ecr" {
  source          = "git::https://github.com/tmknom/terraform-aws-ecr.git?ref=tags/1.0.0"
  name            = "complete"
  tag_prefix_list = ["release"]

  only_pull_accounts       = ["123456789012"]
  push_and_pull_accounts   = ["111111111111"]
  max_untagged_image_count = 5
  max_tagged_image_count   = 50
}
```

## Examples

- [Minimal](https://github.com/tmknom/terraform-aws-ecr/tree/master/examples/minimal)
- [Complete](https://github.com/tmknom/terraform-aws-ecr/tree/master/examples/complete)

## Inputs

| Name                     | Description                                                                  |  Type  | Default | Required |
| ------------------------ | ---------------------------------------------------------------------------- | :----: | :-----: | :------: |
| name                     | Name of the repository.                                                      | string |    -    |   yes    |
| tag_prefix_list          | List of image tag prefixes on which to take action with lifecycle policy.    |  list  |    -    |   yes    |
| max_tagged_image_count   | The maximum number of tagged images that you want to retain in repository.   | string |  `30`   |    no    |
| max_untagged_image_count | The maximum number of untagged images that you want to retain in repository. | string |   `1`   |    no    |
| only_pull_accounts       | AWS accounts which pull only.                                                |  list  |  `[]`   |    no    |
| push_and_pull_accounts   | AWS accounts which push and pull.                                            |  list  |  `[]`   |    no    |

## Outputs

| Name                       | Description                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------- |
| ecr_repository_arn         | Full ARN of the repository.                                                                        |
| ecr_repository_name        | The name of the repository.                                                                        |
| ecr_repository_registry_id | The registry ID where the repository was created.                                                  |
| ecr_repository_url         | The URL of the repository (in the form aws_account_id.dkr.ecr.region.amazonaws.com/repositoryName) |

## Development

### Development Requirements

- [Docker](https://www.docker.com/)

### Configure environment variables

```shell
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
export AWS_DEFAULT_REGION=ap-northeast-1
```

### Installation

```shell
git clone git@github.com:tmknom/terraform-aws-ecr.git
cd terraform-aws-ecr
make install
```

### Makefile targets

```text
apply-complete                 Run terraform apply examples/complete
apply-minimal                  Run terraform apply examples/minimal
bump-version                   Bump version (Required argument 'VERSION')
check-format                   Check format code
clean                          Clean .terraform
destroy-complete               Run terraform destroy examples/complete
destroy-minimal                Run terraform destroy examples/minimal
diff                           Word diff
docs                           Generate docs
format                         Format code
help                           Show help
install                        Install requirements
lint                           Lint code
plan-complete                  Run terraform plan examples/complete
plan-minimal                   Run terraform plan examples/minimal
release                        Release GitHub and Terraform Module Registry
upgrade                        Upgrade makefile
```

### Releasing new versions

Bump VERSION file, and run `make release`.

### Terraform Module Registry

- <https://registry.terraform.io/modules/tmknom/ecr/aws>

## License

Apache 2 Licensed. See LICENSE for full details.
