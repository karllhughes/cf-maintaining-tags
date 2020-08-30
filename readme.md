# CloudForecast - Maintaining Tags

This repo contains demo code from the third part of the CloudForecast series on tagging resources in AWS. The subdirectories in this repository contain sample policies for auditing and maintaining existing tags.

Detailed instructions can be found in the blog post.

## CloudCustodian

- Install CloudCustodian as [detailed here](https://cloudcustodian.io/docs/quickstart/index.html)
- Find non-compliant resources (using Docker):
```
docker run -it -e AWS_ACCESS_KEY_ID="XXXX" \
	-e AWS_SECRET_ACCESS_KEY="YYYY" \
	-e AWS_DEFAULT_REGION="us-east-2" \
	-v $(pwd)/cloudcustodian:/home/custodian \
	cloudcustodian/c7n run -s /home/custodian/output /home/custodian/policy-report.yml --cache-period=0
```
- Stop non-compliant resources and start compliant ones:
```
docker run -it -e AWS_ACCESS_KEY_ID="XXXX" \
	-e AWS_SECRET_ACCESS_KEY="YYYY" \
	-e AWS_DEFAULT_REGION="us-east-2" \
	-v $(pwd)/cloudcustodian:/home/custodian \
	cloudcustodian/c7n run -s /home/custodian/output /home/custodian/policy-stop.yml --cache-period=0
```

The CLI will give you a count of the number of instances affected, but you can check the `./cloudcustodian/output` directory for detailed reports.

## Gold Fig

- Follow ["Getting Started" instructions](https://github.com/goldfiglabs/goldfig)
- Run `./gf init` to initialize the data schema and `./gf account aws import` to import your AWS data. Output like this:
```
Results - Import #1
  Start:  2020-08-30 15:16:23
  End:  2020-08-30 15:26:53
  Errors:  0
  Resources added:  180
  Resources removed:  0
  Resources updated:  1
    Attributes added:  13
    Attributes removed:  0
    Attributes updated:  0
```
- To see summary of Gold Fig's data: `./gf status`
```
aws - o-bbpp1x41ol
Provider Resources
  aws_cloudtrail_trail:  0
  aws_cloudwatch_compositealarm:  0
  aws_cloudwatch_metricalarm:  0
  aws_config_configurationrecorder:  1
  aws_ec2_flowlog:  0
  aws_ec2_image:  1
  aws_ec2_instance:  3
  aws_ec2_keypair:  2
  aws_ec2_routetable:  17
  aws_ec2_securitygroup:  19
  aws_ec2_subnet:  53
  aws_ec2_volume:  3
  aws_ec2_vpc:  17
  aws_elb_listener:  1
  aws_elb_loadbalancer:  1
  aws_iam_group:  0
  aws_iam_grouppolicy:  0
  aws_iam_instanceprofile:  0
  aws_iam_passwordpolicy:  0
  aws_iam_policy:  6
  aws_iam_policyversion:  44
  aws_iam_role:  6
  aws_iam_rolepolicy:  0
  aws_iam_rootaccount:  1
  aws_iam_signingcertificate:  0
  aws_iam_user:  0
  aws_iam_userpolicy:  0
  aws_kms_key:  0
  aws_lambda_alias:  0
  aws_lambda_function:  0
  aws_lambda_functionversion:  0
  aws_logs_loggroup:  0
  aws_logs_metric:  0
  aws_logs_metricfilter:  0
  aws_organizations_account:  1
  aws_organizations_organization:  1
  aws_organizations_organizationalunit:  0
  aws_organizations_root:  1
  aws_rds_dbcluster:  0
  aws_rds_dbinstance:  0
  aws_s3_bucket:  2
  aws_sns_subscription:  0
  aws_sns_topic:  0
Common Resources
  common_storagebucket:  2
Raw Resources
  resources:  180
  attribrutes:  1718
Latest import
  Start:  2020-08-30 15:16:23
  End:  2020-08-30 15:26:53
  Errors:  0
  Resources added:  180
  Resources removed:  0
  Resources updated:  1
    Attributes added:  13
    Attributes removed:  0
    Attributes updated:  0
```
- Find untagged resources: `./gf tags find-untagged`
- Get a tags report: `./gf tags report`
- Run a query to get EC2 instances missing one of your required tags: 
```
./gf run "SELECT resource_id, tags FROM aws_ec2_instance WHERE NOT (tags @> '[{\"Key\": \"accessLevel\"}]'::jsonb) OR ..."
```

## Auto Tag

## Retro Tag
