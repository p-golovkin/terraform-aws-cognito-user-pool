[<img src="https://raw.githubusercontent.com/mineiros-io/brand/e9fff6ecb9617dcb405079f301e23fd83b79c5f3/mineiros-primary-logo.svg" width="400"/>](https://www.mineiros.io/?ref=terraform-aws-cognito-user-pool)

[![Build Status](https://mineiros.semaphoreci.com/badges/terraform-aws-cognito-user-pool/branches/master.svg?style=shields&key=45df31b5-397b-427e-9b96-b995b0daf7c2)](https://mineiros.semaphoreci.com/projects/terraform-aws-cognito-user-pool)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/mineiros-io/terraform-aws-cognito-user-pool.svg?label=latest&sort=semver)](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/releases)
[![license](https://img.shields.io/badge/license-Apache%202.0-brightgreen.svg)](https://opensource.org/licenses/Apache-2.0)
[![Terraform Version](https://img.shields.io/badge/terraform-~%3E%200.12.20-623CE4.svg)](https://github.com/hashicorp/terraform/releases)
[<img src="https://img.shields.io/badge/slack-@mineiros--community-f32752.svg?logo=slack">](https://join.slack.com/t/mineiros-community/shared_invite/zt-ehidestg-aLGoIENLVs6tvwJ11w9WGg)

# terraform-aws-cognito-user-pool

A [Terraform](https://www.terraform.io) 0.12 module for deploying and managing
[Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)
on [Amazon Web Services (AWS)](https://aws.amazon.com/).

- [Module Features](#module-features)
- [Getting Started](#getting-started)
- [Module Argument Reference](#module-argument-reference)
  - [Module Configuration](#module-configuration)
  - [Top-level Arguments](#top-level-arguments)
    - [Cognito User Pool](#cognito-user-pool)
- [External Documentation](#external-documentation)
- [Module Versioning](#module-versioning)
  - [Backwards compatibility in `0.0.z` and `0.y.z` version](#backwards-compatibility-in-00z-and-0yz-version)
- [About Mineiros](#about-mineiros)
- [Reporting Issues](#reporting-issues)
- [Contributing](#contributing)
- [Makefile Targets](#makefile-targets)
- [License](#license)

## Module Features

In contrast to the plain [`cognito_user_pool`](https://www.terraform.io/docs/providers/aws/r/cognito_user_pool.html)
resource this module has a more secure level of default settings.

While all settings can be customized as needed, best practices are
pre-configured.

- **Default Security Settings**:
  Per default, only administrators are allowed to create user profiles by
  setting `allow_admin_create_user_only` to `true`. This module comes with a
  strong default password policy.

**Standard Cognito Features**:
  Create a Cognito User Pool with pre-configured best practices.

- *Features not yet implemented*:
  [`cognito_user_pool_client`](https://www.terraform.io/docs/providers/aws/r/cognito_user_pool_client.html)
  [`cognito_user_pool_domain`](https://www.terraform.io/docs/providers/aws/r/cognito_user_pool_domain.html)
  [`cognito_user_group`](https://www.terraform.io/docs/providers/aws/r/cognito_user_group.html)
  [`cognito_resource_server`](https://www.terraform.io/docs/providers/aws/r/cognito_resource_server.html)

## Getting Started

Most basic usage just setting required arguments:

```hcl
module "terraform-aws-cognito-user-pool" {
  source = "git@github.com:mineiros-io/terraform-aws-cognito-user-pool.git?ref=v0.0.1"

  name = "application-userpool"
}
```

Advanced usage as found in
[examples/complete/main.tf](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/blob/master/complete/example/main.tf)
setting all required and optional arguments to their default values.

## Module Argument Reference

See
[variables.tf](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/blob/master/variables.tf)
and
[examples/](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/blob/master/examples)
for details and use-cases.

### Module Configuration

- **`module_enabled`**: *(Optional `bool`)*
Specifies whether resources in the module will be created.
Default is `true`.

- **`module_depends_on`**: *(Optional `list(any)`)*
A list of dependencies. Any object can be assigned to this list to define a hidden
external dependency.

### Top-level Arguments

#### Cognito User Pool

- **`name`**: **(Required `string`, Forces new resource)**

  The name of the user pool.

- **`advanced_security_mode`**: *(Optional `string`)*

  The mode for advanced security, must be one of `OFF`, `AUDIT` or `ENFORCED`. Additional pricing applies for Amazon Cognito advanced security features. For details see https://aws.amazon.com/cognito/pricing/.
  Default is `OFF`.

- **`alias_attributes`**: *(Optional `set(string)`)*

  Attributes supported as an alias for this user pool. Possible values: `phone_number`, `email`, or `preferred_username`.
  Default is `["email", "preferred_username",]`.

- **`allow_admin_create_user_only`**: *(Optional `bool`)*

  Set to True if only the administrator is allowed to create user profiles. Set to False if users can sign themselves up via an app.
  Default is `true`.

- **`invite_email_subject`**: *(Optional `string`)*

  The message template for email messages.
  Default is `"Your verification code"`.

- **`invite_email_message`**: *(Optional `string`)*

  The message template for email messages. Must contain {username} and {####} placeholders, for username and temporary password, respectively.
  Default is `"Your username is {username} and your temporary password is '{####}'"`.

- **`invite_sms_message`**: *(Optional `string`)*

  The message template for SMS messages. Must contain {username} and {####} placeholders, for username and temporary password, respectively.
  Default is `"Your username is {username} and your temporary password is '{####}'."`.

- **`auto_verified_attributes`**: *(Optional `set(string)`)*

  The attributes to be auto-verified. Possible values: 'email', 'phone_number'.
  Default is `["email"]`.

- **`challenge_required_on_new_device`**: *(Optional `bool`)*
  Indicates whether a challenge is required on a new device. Only applicable to a new device.
  Default is `true`.

- **`device_only_remembered_on_user_prompt`**: *(Optional `string`)*

  If true, a device is only remembered on user prompt.
  Default is `true`.

- **`enable_username_case_sensitivity`**: *(Optional `bool`)*

  Specifies whether username case sensitivity will be applied for all users in the user pool through Cognito APIs.
  Default is `false`.

- **`email_sending_account`**: *(Optional `string`)*

  The email delivery method to use. `COGNITO_DEFAULT` for the default email functionality built into Cognito or `DEVELOPER` to use your Amazon SES configuration.
  Default is `"COGNITO_DEFAULT"`.

- **`email_reply_to_address`**: *(Optional `string`)*

  The REPLY-TO email address.
  Default is `null`.

- **`email_source_arn`**: *(Optional `string`)*

  The ARN of the email source.
  Default is `null`.

- **`email_from_address`**: *(Optional `string`)*

  Sender’s email address or sender’s name with their email address (e.g. 'john@smith.com' or 'John Smith <john@smith.com>').
  Default is `null`.

- **`mfa_configuration`**: *(Optional `string`)*

  Multi-Factor Authentication (MFA) configuration for the User Pool. Valid values: `ON`, `OFF` or `OPTIONAL`. `ON` and `OPTIONAL` require at least one of `sms_configuration` or `software_token_mfa_configuration` to be configured.
  Default is `"OPTIONAL"`.

- **`password_minimum_length`**: *(Optional `number`)*

  The minimum length of the password policy that you have set.
  Default is `20`.

- **`password_require_lowercase`**: *(Optional `bool`)*

  Whether you have required users to use at least one lowercase letter in their password.
  Default is `true`.

- **`password_require_numbers`**: *(Optional `bool`)*

  Whether you have required users to use at least one number in their password.
  Default is `true`.

- **`password_require_symbols`**: *(Optional `bool`)*

  Whether you have required users to use at least one symbol in their password.
  Default is `true`.

- **`password_require_uppercase`**: *(Optional `bool`)*

  Whether you have required users to use at least one uppercase letter in their password.
  Default is `true`.

- **`temporary_password_validity_days`**: *(Optional `number`)*

  In the password policy you have set, refers to the number of days a temporary password is valid. If the user does not sign in during this time, their password will need to be reset by an administrator.
  Default is `1`.

- **`allow_software_mfa_token`**: *(Optional `bool`)*

  Boolean whether to enable software token Multi-Factor Authentication (MFA) tokens, such as Time-Based One-Time Password (TOTP). To disable software token MFA when `sms_configuration` is not present, the `mfa_configuration` argument must be set to `OFF` and the `software_token_mfa_configuration` configuration block must be fully removed.
  Default is `true`.

- **`sms_authentication_message`**: *(Optional `string`)*

  A string representing the SMS authentication message. The message must contain the `{####}` placeholder, which will be replaced with the authentication code.
  Default is `"Your temporary password is {####}."`.

- **`lambda_create_auth_challenge`**: *(Optional `string`)*

  The ARN of an AWS Lambda creating an authentication challenge.
  Default is `null`.

- **`lambda_custom_message`**: *(Optional `string`)*

  The ARN of a custom message AWS Lambda trigger.
  Default is `null`.

- **`lambda_define_auth_challenge`**: *(Optional `string`)*

  The ARN of an AWS Lambda that defines the authentication challenge.
  Default is `null`.

- **`lambda_post_authentication`**: *(Optional `string`)*

  The ARN of an AWS Lambda that defines the authentication challenge.
  Default is `null`.

- **`lambda_post_confirmation`**: *(Optional `string`)*

  The ARN of a post-confirmation AWS Lambda trigger.
  Default is `null`.

- **`lambda_pre_authentication`**: *(Optional `string`)*

  The ARN of a pre-authentication AWS Lambda trigger.
  Default is `null`.

- **`lambda_pre_sign_up`**: *(Optional `string`)*

  The ARN of a pre-registration AWS Lambda trigger.
  Default is `null`.

- **`lambda_pre_token_generation`**: *(Optional `string`)*

  The ARN of an AWS Lambda that allows customization of identity token claims before token generation.
  Default is `null`.

- **`lambda_user_migration`**: *(Optional `string`)*

  The ARN of he user migration AWS Lambda config type.
  Default is `null`.

- **`lambda_verify_auth_challenge_response`**: *(Optional `string`)*

  The ARN of an AWS Lambda that verifies the authentication challenge response.
  Default is `true`.

- **`schema_attributes`**: *(Optional `any`)*

  A list of schema attributes of a user pool. You can add a maximum um 25 custom
  attributes. Please note that only default attributes can be marked as required.
  Also an attribute cannot be switched between required and not required after a
  user pool has been created.
  For details please see the docs: https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html
  Default is `[]`.

  **Example:**

  ```hcl
    schema_attributes = [
      {
        name = "gender", # overwrites the default attribute 'gender'
        type = "String"
        required = true  # required can only be set for default attributes
      },
      {
        name       = "alternative_name"
        type       = "String"
        min_length = 0,
        max_length = 2048
      },
      {
        name      = "friends_count"
        type      = "Number"
        min_value = 0,
        max_value = 100
      },
      {
        name = "is_active"
        type = "Boolean"
      },
      {
        name = "last_seen"
        type = "DateTime"
      }
    ]
  ```

- **`sms_configuration`**: *(Optional `object({external_id = string, sns_caller_arn = string})`)*

  The `sms_configuration` with the `external_id` parameter used in IAM role trust relationships and the `sns_caller_arn` parameter to set the ARN of the Amazon SNS caller. This is usually the IAM role that you have given AWS Cognito permission to assume.
  Default is `null`.

- **`default_email_option`**: *(Optional `string`)*

  The default email option. Must be either `CONFIRM_WITH_CODE` or `CONFIRM_WITH_LINK`.
  Default is `"CONFIRM_WITH_CODE"`.

- **`email_message`**: *(Optional `string`)*

  The email message template. Must contain the `{####}` placeholder.
  Default is `"Your verification code is {####}."`.

- **`email_message_by_link`**: *(Optional `string`)*

  The email message template for sending a confirmation link to the user, it must contain the `{##Click Here##}` placeholder.
  Default is `"Please click the link below to verify your email address. {##Verify Email##}."`.

- **`email_subject`**: *(Optional `string`)*

  The subject line for the email message template.
  Default is `"Your Verification Code"`.

- **`email_subject_by_link`**: *(Optional `string`)*

  The subject line for the email message template for sending a confirmation link to the user.
  Default is `"Your Verifiction Link"`.

- **`sms_message`**: *(Optional `string`)*

  The SMS message template. Must contain the `{####}` placeholder, which will be replaced with the verification code. Can also contain the `{username}` placeholder which will be replaced with the username.
  Default is `"Your verification code is {####}."`.

- **`tags`**: *(Optional `map(string)`)*

  A mapping of tags to assign to the resource.
  Default is `{}`.

  **Example:**

   ```hcl
     tags = {
       CreatedAt = "2020-02-07",
         Alice   = "Bob"
     }
   ```

- ## Module Attributes Reference

The following attributes are exported by the module:

- **`user_pool`**: The `cognito_user_pool` object.
- **`module_enabled`**: Whether this module is enabled.

## External Documentation

- AWS Documentation:
  - https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html
- Terraform AWS Provider Documentation:
  - https://www.terraform.io/docs/providers/aws/r/cognito_user_pool.html

## Module Versioning

This Module follows the principles of [Semantic Versioning (SemVer)](https://semver.org/).

Using the given version number of `MAJOR.MINOR.PATCH`, we apply the following constructs:

1. Use the `MAJOR` version for incompatible changes.
1. Use the `MINOR` version when adding functionality in a backwards compatible manner.
1. Use the `PATCH` version when introducing backwards compatible bug fixes.

### Backwards compatibility in `0.0.z` and `0.y.z` version

- In the context of initial development, backwards compatibility in versions `0.0.z` is **not guaranteed** when `z` is
  increased. (Initial development)
- In the context of pre-release, backwards compatibility in versions `0.y.z` is **not guaranteed** when `y` is
increased. (Pre-release)

## About Mineiros

Mineiros is a [DevOps as a Service](https://www.mineiros.io/?ref=terraform-aws-cognito-user-pool)
company based in Berlin, Germany. We offer commercial support
for all of our projects and encourage you to reach out if you have any questions or need help.
Feel free to send us an email at [hello@mineiros.io](mailto:hello@mineiros.io) or join our
[Community Slack channel](https://join.slack.com/t/mineiros-community/shared_invite/zt-ehidestg-aLGoIENLVs6tvwJ11w9WGg).

- Terraform Modules for all types of infrastructure such as VPC's, Docker clusters,
databases, logging and monitoring, CI, etc.
- Consulting & Training on AWS, Terraform and DevOps.

## Reporting Issues

We use GitHub [Issues](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/issues)
to track community reported issues and missing features.

## Contributing

Contributions are always encouraged and welcome! For the process of accepting changes, we use
[Pull Requests](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/pulls). If you’d like more information, please
see our [Contribution Guidelines](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/blob/master/CONTRIBUTING.md).

## Makefile Targets

This repository comes with a handy
[Makefile](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/blob/master/Makefile).
Run `make help` to see details on each available target.

## License

This module is licensed under the Apache License Version 2.0, January 2004.
Please see [LICENSE](https://github.com/mineiros-io/terraform-aws-cognito-user-pool/blob/master/LICENSE) for full details.

Copyright &copy; 2020 Mineiros GmbH
