# services.repeater

## Certificate Manager

Create a certificate that has the following:

- client-name.com
- \*.client-name.com

NOTE: There is a prefix slash being added by editor on save if viewing this as a raw README

## S3

### Bucket Creation

1.  Create buckets

    - beta.client-name.com
    - client-name.com

1.  Set public access settings for buckets to allow for all objects to be availbale to public

### Update Properties

In the Properties tab:

1.  Enable static website hosting and populate the values for given fields

    - Index document
      - index.html
    - Error document
      - index.html

1.  Copy the name of the "Bucket website endpoint" as it will be used in the CloudFront distribution.

    Example: http://beta.client-name.com.s3-website-us-east-1.amazonaws.com/

### Update Permissions

In the Permissions tab, add the following in the Bucket Policy section

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::beta.client-name.com/*"
        }
    ]
}
```

## CloudFront

Create new distribution
Origin domain will be populated with Bucket website endpoint" from S3 step above.
Viewer protocol policy
Redirect HTTP to HTTPS
Price class
Use only North America and Europe
Alternate domain name
beta.client-name.com
Custom SSL certificate
Choose the certificate that corresponds with base domain name.
Default root object
Leave blank
Description
beta.client-name.com

## Route 53

Create record
beta or empty
Toggle "Alias"
In first input box, select "Alias to CloudFront distribution"
In second input box, select the single distribution. If no distribution exists, something is wrong.
