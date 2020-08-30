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



## Auto Tag

## Retro Tag
