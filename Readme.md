# "Sorry" page

This static page is a fallback to route traffic to, in the event of an outage or serious app problem
during a Challenge.

It should have no asset dependencies except font & component ones loaded from an external CDN endpoint, which should
be available by virtue of [components](https://github.com/thebiggive/components) being npm-published, and not reliant
on any Big Give infrastructure.

It is permanently available at the CloudFront-fronted endpoint
https://sorry.thebiggive.org.uk for testing. It is also registered as an origin,
normally unused, in the live Donate frontend CloudFront distribution (ID E3M8692L2XFBZY).

## Live domains failover

The infrastructure repository is configured with all the support needed to update either
donor-facing CloudFront distribution. Use its variables and don't try to reconfigure it manually.

Remember that if the underlying issue is brief, doing this **may _increase_ the site’s downtime** for some donors. Use with caution!

To switch Donate to the Sorry page and deactivated its APIs, set GitHub var `TF_VAR_disable_custom_apps`.

To switch WordPress to the Sorry page, set GitHub var `TF_VAR_disable_wordpress`.

See [the infra var-only change notes](https://github.com/thebiggive/infrastructure/blob/main/Readme.md#variable-only-changes) for how to apply the changes.

## Updating page contents

You can edit the messaging in [sorry.html](./sorry.html) and manually upload it to [this bucket](https://s3.console.aws.amazon.com/s3/buckets/tbg-sorry/?region=eu-west-1&tab=overview), making sure to **enable public read access**.

Be sure to commit and push your changes to this repository.

Note that S3 website hosting is turned off since users don’t interact with S3 directly.
