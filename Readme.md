# "Sorry" page

This static page is a fallback to route traffic to, in the event of an outage or serious app problem
during a Challenge.

It should have no asset dependencies except CSS & images loaded from a CloudFront CDN endpoint, which
points at another S3 bucket, and this page itself is hosted statically on S3.

It is permanently available at the CloudFront-fronted endpoint
https://sorry.thebiggive.org.uk for testing. It is also registered as an origin,
normally unused, in the live Donate frontend CloudFront distribution (ID E3M8692L2XFBZY).

## Failover method

Because _donate._ already points to a CloudFront distribution, we cannot reroute to another at DNS level seamlessly, as only one distribution can be configured to support the same CNAME. To keep the CloudFront changes as narrow in scope as possible, there is already a Sorry static S3 site Origin set up on the Donate distributions - it is named **sorry-s3**.

Remember that if the underlying issue is brief, doing this **may _increase_ the site’s downtime** for up to 30 minutes for some donors. Use with caution!

To switch _donate._ main traffic that would normally go through the ALB and on to the ECS cluster over to the Sorry page:
 
 * go to the [Production Distribution](https://console.aws.amazon.com/cloudfront/home?region=eu-west-1#distribution-settings:E3M8692L2XFBZY) > Behaviors
 * in the _Default (*)_ Behavior in the AWS Console, manually change the origin from **donate-alb-ecs** to **sorry-s3**.

Note that S3 website hosting is turned off since users don’t interact with S3 directly.

## Updating the page

You can edit the messaging in [sorry.html](./sorry.html) and manually upload it to [this bucket](https://s3.console.aws.amazon.com/s3/buckets/tbg-sorry/?region=eu-west-1&tab=overview), making sure to enable public read access.

Be sure to commit and push your changes to this repository.

### Alternative method

Switching to another distribution requires slow changes, because multiple CloudFront
distributions cannot support the same hostname at once.

So it is **not recommended** to attempt to failover using DNS with our current setup.

If for some reason you have no other option, you can still do so, and point to the
Sorry CloudFront distribution normally now used only for testing the sorry page content.

That distribution is also configured to serve any would-be-404 - i.e. any request except for `/sorry.html` -
with that page and a 200 status code. See Error Pages in [its settings](https://console.aws.amazon.com/cloudfront/home?region=eu-west-1#distribution-settings:E336APQ2P4ZG2S).
