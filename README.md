# AWS Cloudfront Segment Proxy

On April, 30th 2019, [Segment](https://www.segment.com) released [documentation](https://segment.com/docs/guides/sources/custom-domains/) on how to create a proxy to their service using [AWS Cloudfront](https://aws.amazon.com/cloudfront/). This repository features a [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template which will create the Cloudfront distribution as documented by Segment, and request a free SSL Certificate with a single command.

## Prerequisites

### Setting up the AWS CLI

You’ll need to have the [AWS Command Line Interface](https://aws.amazon.com/cli/) installed and configured to connect to your AWS account. On a Mac with [Homebrew](https://brew.sh/) it's as easy as entering the command:

```sh
$ brew install awscli
```

If you don't have or want to use Homebrew, you’ll need Python 2.6.5 or higher and to install using Pip:

```sh
$ pip install awscli
```

For Windows, download and install the installer from [here](https://aws.amazon.com/cli/).

### Authenticating with AWS

Enter `aws configure` at the command line, and then press Enter.

```sh
$ aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]:
Default output format [None]:
```

If your AWS account requires Multi-Factor Authentication (MFA), then [aws-mfa](https://github.com/broamski/aws-mfa) makes it easy to manage your AWS SDK Security Credentials when MFA is enforced on your AWS account.

## Getting Started

1. Clone this repository

```sh
$ mkdir aws-cloudfront-segment-proxy && cd $_
$ git clone https://github.com/loganbussey/aws-cloudfront-segment-proxy.git .
```

2. Run `setup` and follow the prompts:

```sh
$ ./setup
This script will help you setup a Cloudfront Distribution for a Segment proxy and request a SSL Certificate for the domains that you choose.

Enter your Segment CDN domain name [ENTER]: cdn.example.com
Enter your Segment tracking API domain name [ENTER]: api.example.com
Which validation method would you like to use for your SSL Certificate?
EMAIL
DNS
Which would you like to use? EMAIL
Enter a CloudFormation stack name [segment-proxy] [ENTER]: my-segment-proxy
```

Note: If you choose email as the domain validation method, you will need access to the domain administrators inbox to validate the certificate creation. If you choose `DNS`, then you will need to validate the domain with a `CNAME`.

To check on the status of the stack creation the following AWS services will be useful:
* [Cloudformation](https://console.aws.amazon.com/cloudformation/home)
* [Certificate Manager](https://console.aws.amazon.com/acm/home)
* [Cloudfront](https://console.aws.amazon.com/cloudfront/home)

## Next steps

1. Point your domains to the Cloudfront distributions that were created.

2. Change `cdn.segment.com` to your CDN domain in the Analytics.JS snippet.

3. Contact Segment Support and let them know which domains you used so they can update the apiHost of your code. Alternatively, we've found that you can do this via the load options of the snippet directly on your page. E.g.

```js
analytics.load("yourWriteKey", {
  integrations: {
    "Segment.io": {
      apiHost: "segment.example.com/v1",
    },
  },
});
```

If you need help setting this up or with your Segment implementation in general, get in touch with us at [hello@loganbussey.com](mailto:hello@loganbussey.com), or visit [Logan Bussey](https://www.loganbussey.com/) for more information.
