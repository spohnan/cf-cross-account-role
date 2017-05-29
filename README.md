## Cross Account Role CloudFormation Scripts

These scripts automate the creation and configuration of IAM resources needed
to create a role in an account to which you wish to grant users in another account
access.

Overview of steps: http://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html

* **cross-account-power-users.template** - Create a role that authorizes access to dev users in another account
* **cross-account-admin-users.template** - Create a role that authorizes access to admin users in another account
* **cross-account-users.template** - Create a group whose members can switch roles and access cross account resources

### Notes:

* Resource role is similar to PowerUser which restrictes iam:* but also restricts mutation of audit info
* Config service and CloudTrail are restricted to read-only (can't turn off)
* Audit files in S3 buckets are restricted _as long as you follow the bucket naming convention_

### New Account Setup Procedure

* Create new account using Organizations
* Reset root account password, configure MFA and secure credentials using your break glass procedure
	* https://ACCOUNT_ID.signin.aws.amazon.com/console
	* Sign-in using root account credentials link
	* Forgot your password? link
* Turn on audit services
    * Config and CloudTrail services
    * Use default bucket name of config-bucket-ACCOUNT_ID and cloudtrail-bucket-ACCOUNT_ID so buckets are 
      protected by the CF script
    * Set up replication and/or data expiration on s3 buckets
* Run CF scripts to either grant switch role ability to users (master account) or permit user access to resources
