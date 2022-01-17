# S3-Static-Website-Hosting

# Configuring a s3 static website using a custom domain registered with Route 53<a name="website-hosting-custom-domain-walkthrough"></a>


## Step 1: Register a custom domain with Route 53<a name="website-hosting-custom-domain-walkthrough-domain-registry"></a>

If you don't already have a registered domain name, such as `example.com`, register one with Route 53\. For more information, see [Registering a New Domain](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html) in the *Amazon Route 53 Developer Guide*\. After you register your domain name, you can create and configure your Amazon S3 buckets for website hosting\. 

## Step 2: Create two buckets<a name="root-domain-walkthrough-create-buckets"></a>

To support requests from both the root domain and subdomain, you create two buckets\.
+ **Domain bucket** ‐ `example.com`
+ **Subdomain bucket** ‐ `www.example.com` 

These bucket names must exactly match your domain name\. In this example, the domain name is `example.com`\. You host your content out of the root domain bucket \(`example.com`\)\. You create a redirect request for the subdomain bucket \(`www.example.com`\)\. If someone enters `www.example.com` in their browser, they are redirected to `example.com` and see the content that is hosted in the Amazon S3 bucket with that name\. 

**To create your buckets for website hosting**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create your root domain bucket: 

   1. Choose **Create bucket**\.

   1. Enter the **Bucket name** \(for example, `example.com`\)\.

   1. Choose the Region where you want to create the bucket\. 

      Choose a Region close to you to minimize latency and costs, or to address regulatory requirements\. The Region that you choose determines your Amazon S3 website endpoint\.

   1. To accept the default settings and create the bucket, choose **Create**\.

1. Create your subdomain bucket: 

   1. Choose **Create bucket**\.

   1. Enter the **Bucket name** \(for example, `www.example.com`\)\.

   1. Choose the Region where you want to create the bucket\. 

      Choose a Region close to you to minimize latency and costs, or to address regulatory requirements\. The Region that you choose determines your Amazon S3 website endpoint\. 

   1. To accept the default settings and create the bucket, choose **Create**\.

In the next step, you configure `example.com` for website hosting\. 

## Step 3: Configure your root domain bucket for website hosting<a name="root-domain-walkthrough-configure-bucket-aswebsite"></a>

**To enable static website hosting**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the **Buckets** list, choose the name of the bucket that you want to enable static website hosting for\.

1. Choose **Properties**\.

1. Under **Static website hosting**, choose **Edit**\.

1. Choose **Use this bucket to host a website**\. 

1. Under **Static website hosting**, choose **Enable**\.

1. In **Index document**, enter the file name of the index document, typically `index.html`\. 

1. Choose **Save changes**\.

   Amazon S3 enables static website hosting for your bucket\. At the bottom of the page, under **Static website hosting**, you see the website endpoint for your bucket\.

1. Under **Static website hosting**, note the **Endpoint**\.

   The **Endpoint** is the Amazon S3 website endpoint for your bucket\. After you finish configuring your bucket as a static website, you can use this endpoint to test your website\.

In the next step, you configure your subdomain \(`www.example.com`\) to redirect requests to your domain \(`example.com`\)\. 

## Step 4: Configure your subdomain bucket for website redirect<a name="root-domain-walkthrough-configure-redirect"></a>

**To configure a redirect request**

1. On the Amazon S3 console, in the **Buckets** list, choose your subdomain bucket name \(`www.example.com` in this example\)\.

1. Choose **Properties**\.

1. Under **Static website hosting**, choose **Edit**\.

1. Choose **Redirect requests for an object**\. 

1. In the **Target bucket** box, enter your root domain, for example, **example\.com**\.

1. For **Protocol**, choose **http**\.

1. Choose **Save changes**\.
