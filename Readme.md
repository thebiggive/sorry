# "Sorry" page

This static page is a fallback to route traffic to, in the event of an outage or serious app problem
during a Challenge.

It should have no asset dependencies except CSS & images loaded from a CloudFront CDN endpoint, which
points at another S3 bucket, and this page itself is hosted statically on S3.

It is permanently available at http://tbg-sorry.s3-website-eu-west-1.amazonaws.com/ and the CloudFront
endpoint https://sorry.thebiggive.org.uk. CloudFront is used because AWS limits the names you can point
directly at an S3 hosted website, which prevents us from shifting other subdomains' traffic to this page
in an emergency except via CloudFront.

The CloudFront distribution ID is E336APQ2P4ZG2S and its Domain Name is d2xqiklal9vlni.cloudfront.net.
In the event of an outage, consider updating the DNS record for the affected site to an _ALIAS A_ record
pointing at this distribution. See the `sorry` subdomain's Route 53 configuration for an example to
emulate.

Note that the CloudFront distribution is already configured to respond on any of these 4 Alternate Domain
Names, to make it as quick as possible to switch traffic to it with DNS if necessary:

* `sorry.thebiggive.org.uk` (routing always enabled)
* `thebiggive.org.uk`
* `www.thebiggive.org.uk`
* `secure.thebiggive.org.uk`
