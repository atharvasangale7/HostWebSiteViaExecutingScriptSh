# HostWebSiteViaExecutingScriptSh
HostWebSiteViaExecutingScript

# 🌐 Host a Static Website on AWS via CloudShell (S3 and EC2)

This guide walks you through hosting a static website on AWS using two methods:

1. ✅ Amazon S3 (Static Website Hosting)
2. ✅ Amazon EC2 (with Apache Web Server)

All steps are performed via **AWS CloudShell** — no local setup needed.

---

## 🚀 1. Host Static Website using Amazon S3

### ✅ Step-by-Step Script (CloudShell-ready)

```bash
# Set variables
BUCKET_NAME="my-website-$(date +%s)"
REGION="us-east-1"
INDEX_FILE="index.html"

# Create S3 bucket
aws s3 mb s3://$BUCKET_NAME --region $REGION

# Create index.html
cat << 'EOF' > $INDEX_FILE
<!DOCTYPE html>
<html>
<head><title>My S3 Website</title></head>
<body>
  <h1>Hello from S3 Static Website!</h1>
  <p>This page is hosted on AWS S3 using CloudShell.</p>
</body>
</html>
EOF

# Upload file
aws s3 cp $INDEX_FILE s3://$BUCKET_NAME/

# Enable static website hosting
aws s3 website s3://$BUCKET_NAME/ --index-document index.html --error-document index.html

# Disable public access blocks
aws s3api put-public-access-block \
  --bucket $BUCKET_NAME \
  --public-access-block-configuration \
  BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false

# Apply public-read policy
cat << EOF > bucket-policy.json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::$BUCKET_NAME/*"
  }]
}
EOF

aws s3api put-bucket-policy --bucket $BUCKET_NAME --policy file://bucket-policy.json

# Output the website URL
echo "🎉 Your static website is live at:"
echo "👉 http://$BUCKET_NAME.s3-website-$REGION.amazonaws.com"



```
![image alt](https://github.com/atharvasangale7/HostWebSiteViaExecutingScriptSh/blob/1a1f0813ba45dc0e5dfc071960845053510ccdaf/Screenshot%202025-07-28%20162448.png)
![image alt](https://github.com/atharvasangale7/HostWebSiteViaExecutingScriptSh/blob/a1b42db60128cc7ba6f428e013f70ad935e7ea7c/Screenshot%202025-07-28%20162502.png)
![image alt](https://github.com/atharvasangale7/immigration-System/blob/67b3c5784261d3919b09932e782bdc34572044ca/WhatsApp%20Image%202025-06-10%20at%2012.25.08%20PM%20(1).jpeg)
![image alt](https://github.com/atharvasangale7/immigration-System/blob/272c1ad7c03090de74e91ab7cc12647b385a73de/WhatsApp%20Image%202025-06-10%20at%2012.25.08%20PM.jpeg)
![image alt](https://github.com/atharvasangale7/immigration-System/blob/0fb146f3d42298ba645f4d11b8a6336e6bfc8aef/WhatsApp%20Image%202025-06-10%20at%2012.33.18%20PM.jpeg)
![image alt](https://github.com/atharvasangale7/immigration-System/blob/ba64ba1f2693a35e6cd34b54e7293382808eb22e/WhatsApp%20Image%202025-06-10%20at%2012.33.19%20PM.jpeg)
![image alt](https://github.com/atharvasangale7/immigration-System/blob/3cf80a54670dd568c828e701c1cd59e14cca249b/WhatsApp%20Image%202025-06-10%20at%2012.33.20%20PM%20-%20Copy.jpeg)
