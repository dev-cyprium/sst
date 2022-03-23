---
description: "Docs for the sst.NextjsSite construct in the @serverless-stack/resources package"
---
<!--
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!                                                           !!
!!  This file has been automatically generated, do not edit  !!
!!                                                           !!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
-->
The `NextjsSite` construct is a higher level CDK construct that makes it easy to create a Next.js app. It provides a simple way to build and deploy the site to an S3 bucket; setup a CloudFront CDN for fast content delivery; and configure a custom domain for the website URL.

It also allows you to [automatically set the environment variables](#configuring-environment-variables) in your Next.js app directly from the outputs in your SST app.

## Next.js Features
The `NextjsSite` construct uses the [`@sls-next/lambda-at-edge`](https://github.com/serverless-nextjs/serverless-next.js/tree/master/packages/libs/lambda-at-edge) package from the [`serverless-next.js`](https://github.com/serverless-nextjs/serverless-next.js) project to build and package your Next.js app so that it can be deployed to Lambda@Edge and CloudFront.

:::note
To use the `NextjsSite` construct, you have to install `@sls-next/lambda-at-edge` as a dependency in your `package.json`.

```bash
npm install --save @sls-next/lambda-at-edge
```
:::

Most of the Next.js 11 features are supported, including:

- [Static Site Generation (SSG)](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation): Static pages are served out through the CloudFront CDN.
- [Server Side Rendering (SSR)](https://nextjs.org/docs/basic-features/data-fetching#getserversideprops-server-side-rendering): Server side rendering is performed at CloudFront edge locations using Lambda@Edge.
- [API Routes](https://nextjs.org/docs/api-routes/introduction): API requests are served from CloudFront edge locations using Lambda@Edge.
- [Incremental Static Regeneration (ISR)](https://nextjs.org/docs/basic-features/data-fetching#incremental-static-regeneration): Regeneration is performed using Lambda functions, and the generated pages will be served out through the CloudFront CDN.
- [Image Optimization](https://nextjs.org/docs/basic-features/image-optimization): Images are resized and optimized at CloudFront edge locations using Lambda@Edge.

Next.js 12 features like middleware and AVIF image are not yet supported. You can [read more about the features supported by `serverless-next.js`](https://github.com/serverless-nextjs/serverless-next.js#features). And you can [follow the progress on Next.js 12 support here](https://github.com/serverless-nextjs/serverless-next.js/issues/2016).


## Constructor
```ts
new NextjsSite(scope: Construct, id: string, props: NextjsSiteProps)
```
_Parameters_
- __scope__ [`Construct`](https://docs.aws.amazon.com/cdk/api/v2/docs/constructs.Construct.html)
- __id__ `string`
- __props__ [`NextjsSiteProps`](#nextjssiteprops)

## Examples

### Creating a Next.js app

Deploys a Next.js app in the `path/to/site` directory.

```js
new NextjsSite(this, "NextSite", {
  path: "path/to/site",
});
```

## Properties
An instance of `NextjsSite` has the following properties.
### bucketArn

_Type_ : `string`

The ARN of the internally created CDK `Bucket` instance.

### bucketName

_Type_ : `string`

The name of the internally created CDK `Bucket` instance.

### customDomainUrl

_Type_ : `undefined`&nbsp; | &nbsp;`string`

If the custom domain is enabled, this is the URL of the website with the custom domain.

### distributionDomain

_Type_ : `string`

The domain name of the internally created CDK `Distribution` instance.

### distributionId

_Type_ : `string`

The ID of the internally created CDK `Distribution` instance.

### imageCachePolicyProps

_Type_ : [`CachePolicyProps`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.CachePolicyProps.html)

The default CloudFront cache policy properties for images.

### lambdaCachePolicyProps

_Type_ : [`CachePolicyProps`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.CachePolicyProps.html)

The default CloudFront cache policy properties for Lambda@Edge.

### staticCachePolicyProps

_Type_ : [`CachePolicyProps`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.CachePolicyProps.html)

The default CloudFront cache policy properties for static pages.

### url

_Type_ : `string`

The CloudFront URL of the website.


### cdk.bucket

_Type_ : [`Bucket`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.Bucket.html)

The internally created CDK `Bucket` instance.

### cdk.certificate?

_Type_ : [`ICertificate`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.ICertificate.html)

The AWS Certificate Manager certificate for the custom domain.

### cdk.distribution

_Type_ : [`Distribution`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.Distribution.html)

The internally created CDK `Distribution` instance.

### cdk.hostedZone?

_Type_ : [`IHostedZone`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.IHostedZone.html)

The Route 53 hosted zone for the custom domain.

### cdk.regenerationQueue

_Type_ : [`Queue`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.Queue.html)

The internally created CDK `Queue` instance.


## Methods
An instance of `NextjsSite` has the following methods.
### attachPermissions

```ts
attachPermissions(permissions: Permissions)
```
_Parameters_
- __permissions__ [`Permissions`](Permissions)


Attaches the given list of permissions to allow the Next.js API routes and Server Side rendering `getServerSideProps` to access other AWS resources.

#### Examples

### Attaching permissions

```js {5}
const site = new NextjsSite(this, "Site", {
  path: "path/to/site",
});

site.attachPermissions(["sns"]);
```

## NextjsSiteProps


### customDomain?

_Type_ : `string`&nbsp; | &nbsp;[`BaseSiteDomainProps`](BaseSiteDomainProps)

The customDomain for this website. SST supports domains that are hosted either on [Route 53](https://aws.amazon.com/route53/) or externally.
Note that you can also migrate externally hosted domains to Route 53 by [following this guide](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/MigratingDNS.html).

#### Examples

### Configuring custom domains

You can configure the website with a custom domain hosted either on [Route 53](https://aws.amazon.com/route53/) or [externally](#configuring-externally-hosted-domain).

#### Using the basic config (Route 53 domains)

```js {3}
new NextjsSite(this, "Site", {
  path: "path/to/site",
  customDomain: "domain.com",
});
```

#### Redirect www to non-www (Route 53 domains)

```js {3-6}
new NextjsSite(this, "Site", {
  path: "path/to/site",
  customDomain: {
    domainName: "domain.com",
    domainAlias: "www.domain.com",
  },
});
```

#### Configuring domains across stages (Route 53 domains)

```js {7-10}
export default class MyStack extends Stack {
  constructor(scope, id, props) {
    super(scope, id, props);

    new NextjsSite(this, "Site", {
      path: "path/to/site",
      customDomain: {
        domainName:
          scope.stage === "prod" ? "domain.com" : `${scope.stage}.domain.com`,
        domainAlias: scope.stage === "prod" ? "www.domain.com" : undefined,
      },
    });
  }
}
```

#### Using the full config (Route 53 domains)

```js {3-7}
new NextjsSite(this, "Site", {
  path: "path/to/site",
  customDomain: {
    domainName: "domain.com",
    domainAlias: "www.domain.com",
    hostedZone: "domain.com",
  },
});
```

#### Importing an existing certificate (Route 53 domains)

```js {7}
import { Certificate } from "aws-cdk-lib/aws-certificatemanager";

new NextjsSite(this, "Site", {
  path: "path/to/site",
  customDomain: {
    domainName: "domain.com",
    certificate: Certificate.fromCertificateArn(this, "MyCert", certArn),
  },
});
```

Note that, the certificate needs be created in the `us-east-1`(N. Virginia) region as required by AWS CloudFront.

#### Specifying a hosted zone (Route 53 domains)

If you have multiple hosted zones for a given domain, you can choose the one you want to use to configure the domain.

```js {7-10}
import { HostedZone } from "aws-cdk-lib/aws-route53";

new NextjsSite(this, "Site", {
  path: "path/to/site",
  customDomain: {
    domainName: "domain.com",
    hostedZone: HostedZone.fromHostedZoneAttributes(this, "MyZone", {
      hostedZoneId,
      zoneName,
    }),
  },
});
```

#### Configuring externally hosted domain

```js {5-8}
import { Certificate } from "aws-cdk-lib/aws-certificatemanager";

new NextjsSite(this, "Site", {
  path: "path/to/site",
  customDomain: {
    isExternalDomain: true,
    domainName: "domain.com",
    certificate: Certificate.fromCertificateArn(this, "MyCert", certArn),
  },
});
```

Note that the certificate needs be created in the `us-east-1`(N. Virginia) region as required by AWS CloudFront, and validated. After the `Distribution` has been created, create a CNAME DNS record for your domain name with the `Distribution's` URL as the value. Here are more details on [configuring SSL Certificate on externally hosted domains](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/CNAMEs.html).

Also note that you can also migrate externally hosted domains to Route 53 by [following this guide](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/MigratingDNS.html).



### defaults.function.memorySize?

_Type_ : `number`

### defaults.function.permissions?

_Type_ : [`Permissions`](Permissions)

### defaults.function.timeout?

_Type_ : `number`


The default function props to be applied to all the Lambda Functions created by this construct.

#### Examples

### Configuring the Lambda Functions

Configure the internally created CDK [`Lambda Function`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_lambda.Function.html) instance.

```js {4-8}
new NextjsSite(this, "Site", {
  path: "path/to/site",
  defaults: {
    function: {
      timeout: 20,
      memorySize: 2048,
      permissions: ["sns"],
    }
  },
});
```


### disablePlaceholder?

_Type_ : `boolean`

When running `sst start`, a placeholder site is deployed. This is to ensure that the site content remains unchanged, and subsequent `sst start` can start up quickly.




An object with the key being the environment variable name.

#### Examples

### Configuring environment variables

The `NextjsSite` construct allows you to set the environment variables in your Next.js app based on outputs from other constructs in your SST app. So you don't have to hard code the config from your backend. Let's look at how.

Next.js supports [setting build time environment variables](https://nextjs.org/docs/basic-features/environment-variables). In your JS files this looks like:


```js title="pages/index.js"
console.log(process.env.API_URL);
console.log(process.env.USER_POOL_CLIENT);
```

You can pass these in directly from the construct.

```js {3-6}
new NextjsSite(this, "NextSite", {
  path: "path/to/site",
  environment: {
    API_URL: api.url,
    USER_POOL_CLIENT: auth.cognitoUserPoolClient.userPoolClientId,
  },
});
```

Where `api.url` or `auth.cognitoUserPoolClient.userPoolClientId` are coming from other constructs in your SST app.

#### While deploying

On `sst deploy`, the environment variables will first be replaced by placeholder values, `{{ API_URL }}` and `{{ USER_POOL_CLIENT }}`, when building the Next.js app. And after the referenced resources have been created, the Api and User Pool in this case, the placeholders in the HTML and JS files will then be replaced with the actual values.

:::caution
Since the actual values are determined at deploy time, you should not rely on the values at build time. For example, you cannot fetch from `process.env.API_URL` inside `getStaticProps()` at build time.

There are a couple of work arounds:
- Hardcode the API URL
- Read the API URL dynamically at build time (ie. from an SSM value)
- Use [fallback pages](https://nextjs.org/docs/basic-features/data-fetching#fallback-pages) to generate the page on the fly
:::

#### While developing

To use these values while developing, run `sst start` to start the [Live Lambda Development](../live-lambda-development.md) environment.

``` bash
npx sst start
```

Then in your Next.js app to reference these variables, add the [`sst-env`](../packages/static-site-env.md) package.

```bash
npm install --save-dev @serverless-stack/static-site-env
```

And tweak the Next.js `dev` script to:

```json title="package.json" {2}
"scripts": {
  "dev": "sst-env -- next dev",
  "build": "next build",
  "start": "next start"
},
```

Now you can start your Next.js app as usual and it'll have the environment variables from your SST app.

``` bash
npm run dev
```

There are a couple of things happening behind the scenes here:

1. The `sst start` command generates a file with the values specified by the `NextjsSite` construct's `environment` prop.
2. The `sst-env` CLI will traverse up the directories to look for the root of your SST app.
3. It'll then find the file that's generated in step 1.
4. It'll load these as environment variables before running the start command.

:::note
`sst-env` only works if the Next.js app is located inside the SST app or inside one of its subdirectories. For example:

```
/
  sst.json
  nextjs-app/
```
:::

### path

_Type_ : `string`

Path to the directory where the website source is located.

### waitForInvalidation?

_Type_ : `boolean`

While deploying, SST waits for the CloudFront cache invalidation process to finish. This ensures that the new content will be served once the deploy command finishes. However, this process can sometimes take more than 5 mins. For non-prod environments it might make sense to pass in `false`. That'll skip waiting for the cache to invalidate and speed up the deploy process.


### cdk.bucket?

_Type_ : [`BucketProps`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.BucketProps.html)

Pass in bucket information to override the default settings this construct uses to create the CDK Bucket internally.


### cdk.cachePolicies.imageCachePolicy?

_Type_ : [`ICachePolicy`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.ICachePolicy.html)

### cdk.cachePolicies.lambdaCachePolicy?

_Type_ : [`ICachePolicy`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.ICachePolicy.html)

### cdk.cachePolicies.staticCachePolicy?

_Type_ : [`ICachePolicy`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.ICachePolicy.html)


Override the default CloudFront cache policies created internally.

#### Examples

### Reusng CloudFront cache policies

CloudFront has a limit of 20 cache policies per AWS account. This is a hard limit, and cannot be increased. Each `NextjsSite` creates 3 cache policies. If you plan to deploy multiple Next.js sites, you can have the constructs share the same cache policies by reusing them across sites.

```js
import * as cloudfront from "aws-cdk-lib/aws-cloudfront";

const cachePolicies = {
  staticCachePolicy: new NextjsSite(this, "StaticCache", NextjsSite.staticCachePolicyProps),
  imageCachePolicy: new NextjsSite(this, "ImageCache", NextjsSite.imageCachePolicyProps),
  lambdaCachePolicy: new NextjsSite(this, "LambdaCache", NextjsSite.lambdaCachePolicyProps),
};

new NextjsSite(this, "Site1", {
  path: "path/to/site1",
  cfCachePolicies: cachePolicies,
});

new NextjsSite(this, "Site2", {
  path: "path/to/site2",
  cfCachePolicies: cachePolicies,
});
```

### cdk.distribution?

_Type_ : [`BaseSiteCdkDistributionProps`](BaseSiteCdkDistributionProps)

Pass in a value to override the default settings this construct uses to create the CDK `Distribution` internally.

### cdk.regenerationQueue?

_Type_ : [`QueueProps`](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.QueueProps.html)

Override the default settings this construct uses to create the CDK `Queue` internally.
