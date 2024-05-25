#### URLs for the APIs
AWS AppRunner automatically assigns URLs to new services, that cannot be manually set.
After creation, custom domains can be linked.
We provide a [script](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/scripts/link_custom_domain.py) to do this without accessing the registrar's website nor the AWS console.
This script works for (sub-)domains registered on AWS Route53 and GoDaddy only.

For static linking without accessing the registrar's settings after each re-deployment of the AppRunner services, we recommend registering a sub-domain with Route53.
This can be done from the AWS console, under `Route53 > Hosted zones`.
After that, custom domains can be easily linked either from the console, or using the provided [script](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/scripts/link_custom_domain.py).
