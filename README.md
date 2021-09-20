# Honest argoflow-aws Fork

This is the Honest fork of the [argoflow-aws](https://github.com/honestbank/argoflow-aws) project.

## Updating values in `setup.conf`

This section should walk you through which values you need to update in [setup.conf](/setup.conf) and
where to get those values from.

## Infrastructure Setup

### `oauth2-proxy`

#### `client_id` and `client_secret`

Grab the Oauth `client_id` and `client_secret` from the GCP console:

* [test](https://console.cloud.google.com/apis/credentials/oauthclient/883210430459-sbbhktk32ktusjpia2215psv4ghermdl.apps.googleusercontent.com?authuser=0&project=test-api-cloud-infrastructure)
* dev
* qa
* prod

Populate the following Secrets in AWS Secrets Manager:

* `client_id` - `kubeflow/auth/client_id-XXX`
* `client_secret` - `kubeflow/auth/client_secret-XXX`

#### `cookie_secret`

The `cookie_secret` value is provided as an output of the [kubeflow-infrastructure Terraform Cloud workspaces](https://app.terraform.io/app/honestbank/workspaces?tag=kubeflow).
Locate the `kubeflow_oidc_cookie_secret` output variable in the relevant Terraform Cloud workspace and update
the value into the `kubeflow/auth/cookie_secret-XXX` Secret in AWS Secrets Manager.

### RDS Username and Password

The RDS instance is currently defaulted to a username and password of `kubeflow` - set this into the
following Secrets in AWS Secrets Manager:

* `kubeflow/kubeflow/rds_username-XXX`
* `kubeflow/kubeflow/rds_password-XXX` .

### S3 Credentials

A set of AWS IAM credentials are provided by the [kubeflow-infrastructure Terraform Cloud workspaces](https://app.terraform.io/app/honestbank/workspaces?tag=kubeflow).

### DNS

Add an NS record to the root `honestbank.com` Route 53 zone pointing to the nameservers
of the zone created by the [kubeflow-infrastructure Terraform Cloud workspaces](https://app.terraform.io/app/honestbank/workspaces?tag=kubeflow).
Look for the output named `kubeflow_route53_zone_nameservers`.
