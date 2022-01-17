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

## Step 5: Configure logging for website traffic<a name="root-domain-walkthrough-configure-logging"></a>

**To enable server access logging for your root domain bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the same Region where you created the bucket that is configured as a static website, create a bucket for logging, for example `logs.example.com`\.

1. Create a folder for the server access logging log files \(for example, `logs`\)\.

1. \(Optional\) If you want to use CloudFront to improve your website performance, create a folder for the CloudFront log files \(for example, `cdn`\)\.

1. In the **Buckets** list, choose your root domain bucket\.

1. Choose **Properties**\.

1. Under **Server access logging**, choose **Edit**\.

1. Choose **Enable**\.

1. Under the **Target bucket**, choose the bucket and folder destination for the server access logs:
   + Browse to the folder and bucket location:

     1. Choose **Browse S3**\.

     1. Choose the bucket name, and then choose the logs folder\. 

     1. Choose **Choose path**\.
   + Enter the S3 bucket path, for example, `s3://logs.example.com/logs/`\.

1. Choose **Save changes**\.

   In your log bucket, you can now access your logs\. Amazon S3 writes website access logs to your log bucket every 2 hours\.

## Step 6: Upload index and website content<a name="upload-website-content"></a>

**To configure the index document**

1. Create an `index.html` file\.

   If you don't have an `index.html` file, you can use the following HTML to create one:

   ```
   <html xmlns="http://www.w3.org/1999/xhtml" >
   <head>
       <title>My Website Home Page</title>
   </head>
   <body>
     <h1>Welcome to my website</h1>
     <p>Now hosted on Amazon S3!</p>
   </body>
   </html>
   ```

1. Save the index file locally with the *exact* index document name that you entered when you enabled static website hosting for your bucket \(for example, `index.html`\)\.

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the **Buckets** list, choose the name of the bucket that you want to use to host a static website\.

1. Enable static website hosting for your bucket, and enter the exact name of your index document \(for example, `index.html`\)\.
1. To upload the index document to your bucket, do one of the following:
   + Drag and drop the index file into the console bucket listing\.
   + Choose **Upload**, and follow the prompts to choose and upload the index file\.

## Step 7: Edit S3 Block Public Access settings<a name="root-domain-walkthrough-configure-bucket-permissions"></a>

By default, Amazon S3 blocks public access to your account and buckets\. If you want to use a bucket to host a static website, you can use these steps to edit your block public access settings\. 

**Warning**  
Before you complete this step, review [Using Amazon S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html) to ensure that you understand and accept the risks involved with allowing public access\. When you turn off block public access settings to make your bucket public, anyone on the internet can access your bucket\. We recommend that you block all public access to your buckets\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the name of the bucket that you have configured as a static website\.

1. Choose **Permissions**\.

1. Under **Block public access \(bucket settings\)**, choose **Edit**\.

1. Clear **Block *all* public access**, and choose **Save changes**\.

## Step 8: Attach a bucket policy<a name="add-bucket-policy-root-domain"></a>

**Important**  
The following policy is an example only and allows full access to the contents of your bucket\. Before you proceed with this step, review [How can I secure the files in my Amazon S3 bucket?](https://aws.amazon.com/premiumsupport/knowledge-center/secure-s3-resources/) to ensure that you understand the best practices for securing the files in your S3 bucket and risks involved in granting public access\.

1. Under **Buckets**, choose the name of your bucket\.

1. Choose **Permissions**\.

1. Under **Bucket Policy**, choose **Edit**\.

1. To grant public read access for your website, copy the following bucket policy, and paste it in the **Bucket policy editor**\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": [
                   "s3:GetObject"
               ],
               "Resource": [
                   "arn:aws:s3:::example.com/*"
               ]
           }
       ]
   }
   ```

1. Update the `Resource` to your bucket name\.

1. Choose **Save changes**\.

## Step 9: Test your domain endpoint<a name="root-domain-walkthrough-test-website"></a>

After you configure your domain bucket to host a public website, you can test your endpoint\. You will only be able to test the endpoint for your domain bucket since your subdomain bucket is set up for website redirect and not static website hosting\. 

**Note**  
Amazon S3 does not support HTTPS access to the website\. If you want to use HTTPS, you can use Amazon CloudFront to serve a static website hosted on Amazon S3\.  
For more information, see [How do I use CloudFront to serve a static website hosted on Amazon S3?](https://adamtheautomator.com/aws-s3-static-ssl-website/) 

1. Under **Buckets**, choose the name of your bucket\.

1. Choose **Properties**\.

1. At the bottom of the page, under **Static website hosting**, choose your **Bucket website endpoint**\.

## Step 10: Add alias records for your domain and subdomain<a name="root-domain-walkthrough-add-record-to-hostedzone"></a>

### Add an alias record for your root domain and subdomain<a name="add-alis-record"></a>

**To add an alias record for your root domain \(`example.com`\)**

1. Open the Route 53 console at [https://console\.aws\.amazon\.com/route53/](https://console.aws.amazon.com/route53/)\.
**Note**  
If you don't already use Route 53, see [Step 1: Register a Domain](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/getting-started.html#getting-started-find-domain-name) in the *Amazon Route 53 Developer Guide*\. After completing your setup, you can resume the instructions\.

1. Choose **Hosted zones**\.

1. In the list of hosted zones, choose the name of the hosted zone that matches your domain name\.

1. Choose **Create record**\.

1. Choose **Simple routing**, and choose **Next**\.

1. Choose **Define simple record**\.

1. In **Record name** accept the default value, which is the name of your hosted zone and your domain\.

1. In **Value/Route traffic to**, choose **Alias to S3 website endpoint**\.

1. Choose the Region\.

1. Choose the S3 bucket\.

1. In **Record type**, choose **A ‐ Routes traffic to an IPv4 address and some AWS resources**\.

1. For **Evaluate target health**, choose **No**\.

1. Choose **Define simple record**\.

**To add an alias record for your subdomain \(`www.example.com`\)**

1. In the hosted zone for your root domain \(`example.com`\), choose **Create record**\.

1. In **Record name** for your subdomain, type `www`\.

1. In **Value/Route traffic to**, choose **Alias to S3 website endpoint**\.

1. Choose the Region\.

1. Choose the S3 bucket, for example `example.com (s3-website-us-west-2.amazonaws.com)`\.

1. In **Record type**, choose **A ‐ Routes traffic to an IPv4 address and some AWS resources**\.

1. For **Evaluate target health**, choose **No**\.

1. Choose **Define simple record**\.

## Step 11: Test the website<a name="root-domain-testing"></a>

Verify that the website and the redirect work correctly\. In your browser, enter your URLs\. In this example, you can try the following URLs:
+ **Domain** \(`http://example.com`\) – Displays the index document in the `example.com` bucket\.
+ **Subdomain **\(`http://www.example.com`\) – Redirects your request to `http://example.com`\. You see the index document in the `example.com` bucket\.

If your website or redirect links don't work, you can try the following:
+ **Clear cache**–Clear the cache of your web browser\.
+ **Check name servers**–If your web page and redirect links don't work after you've cleared your cache, you can compare the name servers for your domain and the name servers for your hosted zone\. If the name servers don't match, you might need to update your domain name servers to match those listed under your hosted zone\.
