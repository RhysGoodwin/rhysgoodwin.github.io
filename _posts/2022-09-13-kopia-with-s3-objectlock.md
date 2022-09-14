---
title: 'Kopia Ransom Protection with AWS S3 Object Lock'
date: '2022-09-13T00:00:00+00:00'

author_profile: true
excerpt: 'In Windows 7 and Windows 10 disabling IPv6 also disables dynamic DNS registration. I guess it''s a bug?'
layout: single
permalink: /disabling-ipv6-also-disables-dynamic-dns-registration/

embed:
    - ''
url:
    - ''
lightbox-url:
    - ''
testimonial:
    - ''
testimonial_author:
    - ''
categories:
    - Uncategorized
tags:
    - Kopia
    - 's3'
    - 'objectlock'
    - 'WORM'
---

Warning: This article is a work in progress, take it with a grain of salt.
{: .notice--danger}

Kia ora folks,

<a href="https://kopia.io//" target="_blank">Kopia</a> is an excellet open-source backup tool. It’s seldom you come across such a well architected and robust piece of software. The point of this post is to describe using Kopia in conjunction with Amazon S3 with Object Lock to protect against a ransom attack. 

The concern is that if an attacker compromises your host, they not only encrypt your data and demand a ransom, but they also trash your backups. Where tapes can provide an air gap to protect backups, when using AWS S3 storage as a backup target it’s possible to apply a retention lock on objects (object lock) which prevents and object from being deleted or altered for a set period.

I’m new to Kopia, and backup is not a strong point for me so I’m hoping others in the community can provide feedback to enhance this article and approach.  

First, we’ll see AWS S3 versioning and S3 Object Lock in action, then we’ll look at the Kopia setup and simulate an attack and recovery.


# S3 Bucket Setup

I create my buckets and associated objects (IAM accounts etc.) using Terraform. I don't know all the GUI steps, but basically you need:
* An S3 bucket: 
    * Versioning and object lock enabled these settings can only be set at bucket creation 
        * Note, we are not setting a default retention rule, instead we'll allow Kopia to set the retention period (more on that later)
    * All public access is disabled
    * (optional) server side encryption enabled (with bucket key). Maybe overkill since Kopia encrypts the data
    * A lifecycle rule which expires (deletes) non-current data after 30 days
* An IAM account/key with a policy attached with limited permission just to this bucket. Permissions:
   * s3:GetObjectRetention
   * s3:DeleteObjectVersion
   * s3:ListBucketVersions
   * s3:GetObjectAttributes
   * s3:ListBucket
   * s3:PutObjectLegalHold
   * s3:GetObjectLegalHold
   * s3:GetObjectVersionAttributes
   * s3:PutObject
   * s3:GetObject
   * s3:PutObjectRetention
   * s3:DeleteObject
   * s3:GetObjectVersion

Todo: are these permissions^ correct? Where did I get this list from?
{: .notice--danger}

You should be able to tell the settings you need from this TF file:  

<details>  <summary>bkp_s3.tf</summary>
{% highlight terraform %}

resource "aws_s3_bucket" "bkp_s3" {
  bucket = "my-awesome-bucket-name"
  object_lock_enabled = true
  lifecycle {
   prevent_destroy = true #comment this to be able to delete the bucket if you ware really sure. 
   #Ingoring changes here because lifecycle and encryption can be configured on the resource (old way) or with a sub-resource (the new way, which is used here). 
   #Without this ignore things change with every apply this will probably be fixed in a future version. 
   ignore_changes = [
    lifecycle_rule,
    server_side_encryption_configuration
    ]
  }
}

resource "aws_s3_bucket_acl" "bkp_s3"  {
  bucket = aws_s3_bucket.bkp_s3.id
  acl    = "private"
}

resource "aws_s3_bucket_public_access_block" "bkp_s3" {
  bucket                  = aws_s3_bucket.bkp_s3.id
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

resource "aws_s3_bucket_server_side_encryption_configuration" "bkp_s3" {
  bucket = aws_s3_bucket.bkp_s3.bucket
  rule {
    bucket_key_enabled = true
    apply_server_side_encryption_by_default {
      sse_algorithm     = "aws:kms"
    }
  }
}

resource "aws_s3_bucket_versioning" "bkp_s3" {
  bucket = aws_s3_bucket.bkp_s3.id
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_lifecycle_configuration" "bkp_s3" {
  depends_on = [aws_s3_bucket_versioning.bkp_s3]
  bucket = aws_s3_bucket.bkp_s3.id
  rule {
    id      = "Expire Data"
    noncurrent_version_expiration {
      noncurrent_days = 30
    }
    status = "Enabled"
  }
}

resource "aws_iam_user" "bkp_s3" {
  name = "${aws_s3_bucket.bkp_s3.bucket}"
  tags = {
    tag-key = "tag-value"
  }
}

resource "aws_iam_access_key" "bkp_s3" {
  user = aws_iam_user.bkp_s3.name
}

resource "aws_iam_user_policy" "bkp_s3" {
  name = "${aws_s3_bucket.bkp_s3.bucket}"
  user = aws_iam_user.bkp_s3.name

  policy = jsonencode({
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:GetObjectRetention",
                "s3:DeleteObjectVersion",
                "s3:ListBucketVersions",
                "s3:GetObjectAttributes",
                "s3:ListBucket",
                "s3:PutObjectLegalHold",
                "s3:GetObjectLegalHold",
                "s3:GetObjectVersionAttributes",
                "s3:PutObject",
                "s3:GetObject",
                "s3:PutObjectRetention",
                "s3:DeleteObject",
                "s3:GetObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::${aws_s3_bucket.bkp_s3.bucket}",
                "arn:aws:s3:::${aws_s3_bucket.bkp_s3.bucket}/*"
            ]
        }
    ]
 })
}

resource "local_file" "bkp_s3" {
content  = <<EOT
sec_tf_bkp_bucket_iam_id_bkp_s3: ${aws_iam_access_key.bkp_s3.id}
sec_tf_bkp_bucket_iam_secret_bkp_s3: ${aws_iam_access_key.bkp_s3.secret}
EOT
file_permission = 0600
filename = "${var.SecPath}/sec_tf_bkp_iam_bkp_s3.yml"
}   
{% endhighlight %}
</details>   

&nbsp;  

# AWS S3 Versioning and DeleteMarkers
There’s plenty of official doco about S3 versioning. Here I'll step through it in practice. Basically, a documentation of my own experiments.

All of this is done with an AWS IAM credential which has limited access to the S3 bucket (as described above) and is the same credential used to run Kopia backups. Such a credential is most likely to be stolen from a compromised server where backups are configured.


First, I write a file and put it in the bucket:
```bash
rhys@mgmt:~$ echo 'Rerenga tuatahi (first version)' >mytestfile.fun

rhys@mgmt:~$ aws s3 cp ./mytestfile.fun s3://my-awesome-bucket-name/
upload: ./mytestfile.fun to s3://my-awesome-bucket-name/mytestfile.fun
```

I list the file and yes, it's there:
```bash
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name/mytestfile.fun
2022-09-12 23:58:43         32 mytestfile.fun
```

Now I list the object versions for that. Note, there's just 1 version that **IsLatest**:
```bash
rhys@mgmt:~$  aws s3api list-object-versions --bucket my-awesome-bucket-name --prefix mytestfile.fun
{
    "Versions": [
        {
            "ETag": "\"105dd7c965480c0853995fce961f0e73\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "54keir1ROKFGXwT6LWvMM0wAEwcbdyb_",
            "IsLatest": true,
            "LastModified": "2022-09-12T23:58:43+00:00"
        }
    ]
}
```
Now I edit the file and put it in the bucket again (second version). This doesn’t overwrite the original file, it stacks on top of it as the latest version. The first version is now non-current:
```bash
rhys@mgmt:~$ echo 'Rerenga tuarua (second version)' >mytestfile.fun
rhys@mgmt:~$ aws s3 cp ./mytestfile.fun s3://my-awesome-bucket-name/
upload: ./mytestfile.fun to s3://my-awesome-bucket-name/mytestfile.fun
```

Now the third version:
```bash
rhys@mgmt:~$ echo 'Rerenga tuatoru (third version)' >mytestfile.fun
rhys@mgmt:~$ aws s3 cp ./mytestfile.fun s3://my-awesome-bucket-name/
upload: ./mytestfile.fun to s3://my-awesome-bucket-name/mytestfile.fun
```

Now we've got three versions - two which are non-current:
```bash
rhys@mgmt:~$ aws s3api list-object-versions --bucket my-awesome-bucket-name --prefix mytestfile.fun
{
    "Versions": [
        {
            "ETag": "\"188da8a565815139b005b8c331fbaf80\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "YHsF2jhmad6S0tl1azd64241nWdTBv.e",
            "IsLatest": true,
            "LastModified": "2022-09-13T00:34:39+00:00"
        },
        {
            "ETag": "\"0c4ba5107663526195fde438738b437f\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "XurzKINJNUmbgSgr2wKBuYM9yKmHJA5H",
            "IsLatest": false,
            "LastModified": "2022-09-13T00:33:59+00:00"
        },
        {
            "ETag": "\"105dd7c965480c0853995fce961f0e73\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "54keir1ROKFGXwT6LWvMM0wAEwcbdyb_",
            "IsLatest": false,
            "LastModified": "2022-09-12T23:58:43+00:00"
        }
    ]
}
```

Now I "delete” the file with a simple delete:
```bash
rhys@mgmt:~$ aws s3 rm s3://my-awesome-bucket-name/mytestfile.fun
delete: s3://my-awesome-bucket-name/mytestfile.fun
```

Now look at the versions of this object - we have three non-current versions and a DeleteMarker which is the current version (**"IsLatest": true**):
```bash
rhys@mgmt:~$ aws s3api list-object-versions --bucket my-awesome-bucket-name --prefix mytestfile.fun
{
    "Versions": [
        {
            "ETag": "\"188da8a565815139b005b8c331fbaf80\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "YHsF2jhmad6S0tl1azd64241nWdTBv.e",
            "IsLatest": false,
            "LastModified": "2022-09-13T00:34:39+00:00"
        },
        {
            "ETag": "\"0c4ba5107663526195fde438738b437f\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "XurzKINJNUmbgSgr2wKBuYM9yKmHJA5H",
            "IsLatest": false,
            "LastModified": "2022-09-13T00:33:59+00:00"
        },
        {
            "ETag": "\"105dd7c965480c0853995fce961f0e73\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "54keir1ROKFGXwT6LWvMM0wAEwcbdyb_",
            "IsLatest": false,
            "LastModified": "2022-09-12T23:58:43+00:00"
        }
    ],
    "DeleteMarkers": [
        {
            "Key": "mytestfile.fun",
            "VersionId": "ESI2jjpPyXev4Cblf2jxMu0G5no33qEv",
            "IsLatest": true,
            "LastModified": "2022-09-13T00:37:13+00:00"
        }
    ]
}
```

I list the file and sure enough, it's gone. At least it looks like it’s gone:
```bash
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name/mytestfile.fun
[nothing returned]
```

Now I “undelete” the file by deleting the DeleteMarker. Notice I specify the version-id of the DeleteMarker:
```bash
rhys@mgmt:~$ aws s3api delete-object --bucket my-awesome-bucket-name --key mytestfile.fun --version-id "ESI2jjpPyXev4Cblf2jxMu0G5no33qEv"
{
    "DeleteMarker": true,
    "VersionId": "ESI2jjpPyXev4Cblf2jxMu0G5no33qEv"
}
```

See, the DeleteMarker is now gone:
```bash
rhys@mgmt:~$ aws s3api list-object-versions --bucket my-awesome-bucket-name --prefix mytestfile.fun
{
    "Versions": [
        {
            "ETag": "\"188da8a565815139b005b8c331fbaf80\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "YHsF2jhmad6S0tl1azd64241nWdTBv.e",
            "IsLatest": true,
            "LastModified": "2022-09-13T00:34:39+00:00"
        },
        {
            "ETag": "\"0c4ba5107663526195fde438738b437f\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "XurzKINJNUmbgSgr2wKBuYM9yKmHJA5H",
            "IsLatest": false,
            "LastModified": "2022-09-13T00:33:59+00:00"
        },
        {
            "ETag": "\"105dd7c965480c0853995fce961f0e73\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "54keir1ROKFGXwT6LWvMM0wAEwcbdyb_",
            "IsLatest": false,
            "LastModified": "2022-09-12T23:58:43+00:00"
        }
    ]
}
```

And when I list the file it's there again:
```bash
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name/mytestfile.fun
2022-09-13 00:34:39         32 mytestfile.fun
```

What about really deleting a file? Let's try to remove an object for real, rather than just pretending it's gone with a DeleteMarker. This time I use the **aws s3api delete-object** method and specify the version-id of current version:
```bash
rhys@mgmt:~$ aws s3api delete-object --bucket my-awesome-bucket-name --key mytestfile.fun --version-id YHsF2jhmad6S0tl1azd64241nWdTBv.e
{
    "VersionId": "YHsF2jhmad6S0tl1azd64241nWdTBv.e"
}
```

Now the latest version is gone, and the previous version becomes current:
```bash
rhys@mgmt:~$ aws s3api list-object-versions --bucket my-awesome-bucket-name --prefix mytestfile.fun
{
    "Versions": [
        {
            "ETag": "\"0c4ba5107663526195fde438738b437f\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "XurzKINJNUmbgSgr2wKBuYM9yKmHJA5H",
            "IsLatest": true,
            "LastModified": "2022-09-13T00:33:59+00:00"
        },
        {
            "ETag": "\"105dd7c965480c0853995fce961f0e73\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mytestfile.fun",
            "VersionId": "54keir1ROKFGXwT6LWvMM0wAEwcbdyb_",
            "IsLatest": false,
            "LastModified": "2022-09-12T23:58:43+00:00"
        }
    ]
}
```

Now I delete the remaining two versions:
```bash
rhys@mgmt:~$ aws s3api delete-object --bucket my-awesome-bucket-name --key mytestfile.fun --version-id "XurzKINJNUmbgSgr2wKBuYM9yKmHJA5H"
{
    "VersionId": "XurzKINJNUmbgSgr2wKBuYM9yKmHJA5H"
}

rhys@mgmt:~$ aws s3api delete-object --bucket my-awesome-bucket-name --key mytestfile.fun --version-id "54keir1ROKFGXwT6LWvMM0wAEwcbdyb_"
{
    "VersionId": "54keir1ROKFGXwT6LWvMM0wAEwcbdyb_"
}
```

Now the object is 100% gone:
```bash
rhys@mgmt:~$ aws s3api list-object-versions --bucket my-awesome-bucket-name --prefix mytestfile.fun
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name/mytestfile.fun
rhys@mgmt:~$ [nothing returned]
```

Before we move on to looking at Object Lock it’s worth mentioning that *by default* deleted files in an S3 bucket with versioning enabled, that is files which are hidden with a DeleteMarker, are never permanently deleted. This is where our **Lifecycle rule** comes in. It will permanently delete non-current versions *x* days after they become non-current. 

# AWS S3 Object Lock

Let's start again. I create a new file and put it in the bucket:
```bash
rhys@mgmt:~$ echo 'Rerenga tuatahi (first version)' >mysafetestfile.fun
rhys@mgmt:~$ aws s3 cp ./mysafetestfile.fun s3://my-awesome-bucket-name/
upload: ./mysafetestfile.fun to s3://my-awesome-bucket-name/mysafetestfile.fun
```

Now I set an Object Lock retention period:
```bash
rhys@mgmt:~$ aws s3api put-object-retention --bucket my-awesome-bucket-name --key mysafetestfile.fun --retention '{ "Mode": "GOVERNANCE", "RetainUntilDate": "2022-09-16T00:00:00" }'
[nothing returned, success]
```

Using **head-object** we can see the file now has a **RetainUntilDate**:
```bash
rhys@mgmt:~$ aws s3api head-object --bucket my-awesome-bucket-name --key mysafetestfile.fun
{
    "AcceptRanges": "bytes",
    "LastModified": "2022-09-13T00:51:00+00:00",
    "ContentLength": 32,
    "ETag": "\"e2ca77dbc72f1f605256cf9b64910603\"",
    "VersionId": "4KIp9ih15unnz7g3.6v0Wb1F304jCik.",
    "ContentType": "binary/octet-stream",
    "ServerSideEncryption": "aws:kms",
    "Metadata": {},
    "SSEKMSKeyId": "arn:aws:kms:ap-southeast-2:912773030942:key/65945124-c013-451e-a5da-a31d3ce45fb1",
    "BucketKeyEnabled": true,
    "ObjectLockMode": "GOVERNANCE",
    "ObjectLockRetainUntilDate": "2022-09-16T00:00:00+00:00"
}
```

Now let's try a simple delete. Yes it works:
```bash
rhys@mgmt:~$ aws s3 rm s3://my-awesome-bucket-name/mysafetestfile.fun
delete: s3://my-awesome-bucket-name/mysafetestfile.fun
```

Just as before a delete marker is placed:
```bash
rhys@mgmt:~$ aws s3api list-object-versions --bucket my-awesome-bucket-name --prefix mysafetestfile.fun
{
    "Versions": [
        {
            "ETag": "\"e2ca77dbc72f1f605256cf9b64910603\"",
            "Size": 32,
            "StorageClass": "STANDARD",
            "Key": "mysafetestfile.fun",
            "VersionId": "4KIp9ih15unnz7g3.6v0Wb1F304jCik.",
            "IsLatest": false,
            "LastModified": "2022-09-13T00:51:00+00:00"
        }
    ],
    "DeleteMarkers": [
        {
            "Key": "mysafetestfile.fun",
            "VersionId": "tW1o6sPOMoNvu68_lgTnIi8xo6NksxXl",
            "IsLatest": true,
            "LastModified": "2022-09-13T00:57:49+00:00"
        }
    ]
}
```

And AWS acts as if the file is gone:
```bash
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name/mysafetestfile.fun
[nothing returned]
```

Ok so let's delete the DeleteMarker to restore the file:
```bash
rhys@mgmt:~$ aws s3api delete-object --bucket my-awesome-bucket-name --key mysafetestfile.fun --version-id "tW1o6sPOMoNvu68_lgTnIi8xo6NksxXl"
{
    "DeleteMarker": true,
    "VersionId": "tW1o6sPOMoNvu68_lgTnIi8xo6NksxXl"
}
```

And the file is back:
```bash
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name/mysafetestfile.fun
2022-09-13 00:51:00         32 mysafetestfile.fun
```

Now let's try to delete it for real. No go. The Object Lock won't allow it. Good:
```bash
rhys@mgmt:~$ aws s3api delete-object --bucket my-awesome-bucket-name --key mysafetestfile.fun --version-id "4KIp9ih15unnz7g3.6v0Wb1F304jCik."
An error occurred (AccessDenied) when calling the DeleteObject operation: Access Denied
```

Now let's set the retention to a date in the past so we can delete it. Doesn't work. Good:
```bash
rhys@mgmt:~$ aws s3api put-object-retention --bucket my-awesome-bucket-name --key mysafetestfile.fun --retention '{ "Mode": "GOVERNANCE", "RetainUntilDate": "2022-08-16T00:00:00" }'
An error occurred (InvalidArgument) when calling the PutObjectRetention operation: The retain until date must be in the future!
```

What about a retention date that is in the future but only by 1 minute and therefore much earlier than the original retention date? Doesn't work, not allowed. Good: 
```bash
rhys@mgmt:~$ date
Tue 13 Sep 2022 01:06:58 AM UTC

rhys@mgmt:~$ aws s3api put-object-retention --bucket my-awesome-bucket-name --key mysafetestfile.fun --retention '{ "Mode": "GOVERNANCE", "RetainUntilDate": "2022-09-13T01:08:00" }'
An error occurred (AccessDenied) when calling the PutObjectRetention operation: Access Denied
```

What about a date further into the future than the original retention date (by 1 second). That works. So, we can extend the retention date but not reduce it. Good:
```bash
rhys@mgmt:~$ aws s3api put-object-retention --bucket my-awesome-bucket-name --key mysafetestfile.fun --retention '{ "Mode": "GOVERNANCE", "RetainUntilDate": "2022-09-16T00:00:01+00:00" }'
```

Here's the retention date extended by 1 second.
```bash
rhys@mgmt:~$ aws s3api head-object --bucket my-awesome-bucket-name --key mysafetestfile.fun
{
    "AcceptRanges": "bytes",
    "LastModified": "2022-09-13T00:51:00+00:00",
    "ContentLength": 32,
    "ETag": "\"e2ca77dbc72f1f605256cf9b64910603\"",
    "VersionId": "4KIp9ih15unnz7g3.6v0Wb1F304jCik.",
    "ContentType": "binary/octet-stream",
    "ServerSideEncryption": "aws:kms",
    "Metadata": {},
    "SSEKMSKeyId": "arn:aws:kms:ap-southeast-2:912773030942:key/65945124-c013-451e-a5da-a31d3ce45fb1",
    "BucketKeyEnabled": true,
    "ObjectLockMode": "GOVERNANCE",
    "ObjectLockRetainUntilDate": "2022-09-16T00:00:01+00:00"
}
```

From all this we can see how AWS S3 versioning works and how object lock works. 
The retention date for an object can either be specified by the client or it can be set by a default rule for on the bucket with for a specific number of days. Regardless of how retention is set, it is per object.  There are two retention modes available compliance and governance. More info [here](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html#object-lock-retention-modes). I’d recommend using governance during testing then move to compliance later if needed. 

# Kopia and S3 Object Lock

Next up, let's look at this in the context of Kopia and ransom attack protection.

*Most* of these steps can be done in the Kopia’s delightful wee UI. 

First, I create and connect to the repo:
```bash
kopia repository create s3 --bucket=my-awesome-bucket-name --access-key='MeKey' --secret-access-key='MeSecret' --region=ap-southeast-2 --password='mePassword' 
Initializing repository with:
  block hash:          BLAKE2B-256-128
  encryption:          AES256-GCM-HMAC-SHA256
  splitter:            DYNAMIC-4M-BUZHASH
Connected to repository.

NOTICE: Kopia will check for updates on GitHub every 7 days, starting 24 hours after first use.
To disable this behavior, set environment variable KOPIA_CHECK_FOR_UPDATES=false
Alternatively you can remove the file "/home/rhys/.config/kopia/repository.config.update-info.json".

Retention:
  Annual snapshots:                 3   (defined for this target)
  Monthly snapshots:               24   (defined for this target)
  Weekly snapshots:                 4   (defined for this target)
  Daily snapshots:                  7   (defined for this target)
  Hourly snapshots:                48   (defined for this target)
  Latest snapshots:                10   (defined for this target)
  Ignore identical snapshots:   false   (defined for this target)
Compression disabled.

To find more information about default policy run 'kopia policy get'.
To change the policy use 'kopia policy set' command.

NOTE: Kopia will perform quick maintenance of the repository automatically every 1h0m0s
and full maintenance every 24h0m0s when running as rhys@mgmt.

See https://kopia.io/docs/advanced/maintenance/ for more information.

NOTE: To validate that your provider is compatible with Kopia, please run:

$ kopia repository validate-provider
```

Now set the retention on the repo. *(This step can't be done in the UI)*
Note I'm using GOVERNANCE mode during testing because it's more flexible. It still allows admins to delete files, compliance mode locks things up proper
```bash
rhys@mgmt:~$ kopia repo set-parameters --retention-mode=GOVERNANCE --retention-period=90d
 - setting storage backend blob retention mode to GOVERNANCE.

 - setting storage backend blob retention period to 2160h0m0s.

NOTE: Repository parameters updated, you must disconnect and re-connect all other Kopia clients.
```

Now let’s take a snapshot
```bash
rhys@mgmt:~$ kopia snapshot create /home/rhys/
Snapshotting rhys@mgmt:/home/rhys ...
 * 0 hashing, 37356 hashed (2 GB), 0 cached (0 B), uploaded 1.3 GB, estimated 2 GB (100.4%) 0s left
Created snapshot with root k1ec1fe89bba7206a48c6f3a5d7adf2ce and ID 787184263559f186d6c0c68a94ca9a71 in 2m35s
Running full maintenance...
Looking for active contents...
Looking for unreferenced contents...
GC found 0 unused contents (0 B bytes)
GC found 0 unused contents that are too recent to delete (0 B bytes)
GC found 31933 in-use contents (1.3 GiB bytes)
GC found 2 in-use system-contents (1.1 KiB bytes)
Rewriting contents from short packs...
Not enough time has passed since previous successful Snapshot GC. Will try again next time.
Skipping blob deletion because not enough time has passed yet (59m59s left).
Cleaned up 0 logs.
Cleaning up old index blobs which have already been compacted...
Finished full maintenance.
```

Now let's list the files Kopia put in the bucket and examine one of the blobs. You'll notice that Kopia has placed a Object Lock on this object:
```bash
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name
rhys@mgmt:~$ aws s3api head-object --bucket my-awesome-bucket-name --key pfac824779ef631ac2453bab065c36a16-s1d843f2e82e65f72114
{
    "AcceptRanges": "bytes",
    "LastModified": "2022-09-13T02:30:15+00:00",
    "ContentLength": 22839698,
    "ETag": "\"70219fe8ffe10cb89a05b7555ed99909\"",
    "VersionId": "tzcMT4bAF.xumvHIR3DlQztlat0yqPve",
    "ContentType": "application/x-kopia",
    "ServerSideEncryption": "aws:kms",
    "Metadata": {},
    "SSEKMSKeyId": "arn:aws:kms:ap-southeast-2:912773030942:key/65945124-c013-451e-a5da-a31d3ce45fb1",
    "BucketKeyEnabled": true,
    "ObjectLockMode": "GOVERNANCE",
    "ObjectLockRetainUntilDate": "2022-12-12T02:30:13+00:00"
}
```

As things stand when Kopia cleans up data it no longer needs, it does a normal delete which places a DeleteMaker on the object and the data becomes a non-current version, as we saw above. From Kopia's perspective the file is now gone. But the file won't actually be deleted until the lifecycle rule runs.
Our lifecycle rule expires (permanently deletes) the data 30 days after it becomes non-current, **but** this can only happen after the retention period ends. Consider this sequence:
1. Kopia rights a blob with a 90 day retention period
2. 10 days later Kopia deletes the blob, making it a non-current
3. 30 days later the lifecycle rule would expire the data however it still has 50 days of retention to run.
4. 50 days later the retention on the non-current version expires and the lifecycle rule permanently deletes it


Now lets' simulate a ransom attack. Let's presume the server where Kopia is running is compromised and the credential which Kopia uses to write to the bucket is stolen. The attackers encrypt the local server then delete all the objects in the bucket:
```bash
rhys@mgmt:~$ aws s3 rm s3://my-awesome-bucket-name --recursive
delete: s3://my-awesome-bucket-name/kopia.maintenance
delete: s3://my-awesome-bucket-name/_log_20220913015708_bdb8_1663034228_1663034229_1_a67b860863f71d336b05968801324920
delete: s3://my-awesome-bucket-name/_log_20220913022931_471e_1663036171_1663036333_1_982f3c98cdb064bc4b6b255fb0be5c7d
delete: s3://my-awesome-bucket-name/p0b2cbfd1cda6bebc15bdeffa89b8a96e-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/p124bdb974cb7fa91aa99275585329c41-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/p15598f9c78e76b8ec91d1d1d6d35d408-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/p024cc73dc5d7462363743bd7aac20f79-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/p1f3cd580487973337860306225dccb36-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/kopia.blobcfg
delete: s3://my-awesome-bucket-name/kopia.repository
delete: s3://my-awesome-bucket-name/p290fdfc4d8bbe6c50b289c813f43f6fe-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/p25bc556a938b7f76be5170de35521d14-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/p2a1ea3e01a46ddb80bccda64c3424bac-s1d843f2e82e65f72114
delete: s3://my-awesome-bucket-name/p173fdbd7ca15e82bcd125a4f333a263e-s1d843f2e82e65f72114
…
```

At first glance the files are gone:
```bash
rhys@mgmt:~$ aws s3 ls s3://my-awesome-bucket-name
[nothing returned]
```

But on closer inspection we can see they are all still there and we know from our testing above that no snashots created in the last 90 days can not be deleted because of the Object Lock. 

^^This bit I'm not sure about, don't the snapshots created in the last 90 days depend on all the snapshots piror to that? I.e. wouldn't we need an indefinite retention date?
{: .notice--danger}

```bash
rhys@mgmt:~$ aws s3api list-object-versions --bucket my-awesome-bucket-name --query 'DeleteMarkers[?IsLatest==`true`]'
[
    {
        "Key": "_log_20220913015708_bdb8_1663034228_1663034229_1_a67b860863f71d336b05968801324920",
        "VersionId": "Bn2yLzlo0o0xUbyEUijajWbLCR.E7ICj",
        "IsLatest": true,
        "LastModified": "2022-09-13T03:02:21+00:00"
    },
    {
        "Key": "_log_20220913022931_471e_1663036171_1663036333_1_982f3c98cdb064bc4b6b255fb0be5c7d",
        "VersionId": "BVQEbQSKqy.go8ZGG.ciQ9DeAsGiHrO6",
        "IsLatest": true,
        "LastModified": "2022-09-13T03:02:21+00:00"
    },
    {
        "Key": "kopia.blobcfg",
        "VersionId": "NhMwPNYw85C8aN4tnfuX1L4E9PmpPbxw",
        "IsLatest": true,
        "LastModified": "2022-09-13T03:02:21+00:00"
    },
```

Now we have a bucket where every file is marked with a DeleteMarker (non-current) some of that will be legitimate clean-ups by Kopia but most of it is due to ransom attack so ideally we need to undelete (delete the DeleteMarkers) all files deleted after a certain date.


I found [this excellent snippet](https://stackoverflow.com/a/52891667) on StackOverflow which does what we need. This lists DeleteMarkers which were created after a certain date then deletes them. 
```bash
rhys@mgmt:~$ aws s3api delete-objects --bucket my-awesome-bucket-name --no-cli-pager --delete "$(aws s3api list-object-versions --bucket my-awesome-bucket-name --output=json --query='{Objects: DeleteMarkers[?IsLatest==`true` && LastModified>=`2022-09-13T01:00:00+00:00`].{Key:Key,VersionId:VersionId}}')"
```

Now we can see all our objects are visible again:
```bash
rhys@mgmt:~$ aws s3 ls s3:// my-awesome-bucket-name /
2022-09-13 01:57:10        774 _log_20220913015708_bdb8_1663034228_1663034229_1_a67b860863f71d336b05968801324920
2022-09-13 02:32:14    1753420 _log_20220913022931_471e_1663036171_1663036333_1_982f3c98cdb064bc4b6b255fb0be5c7d
2022-09-13 02:26:46         93 kopia.blobcfg
2022-09-13 02:32:14        621 kopia.maintenance
2022-09-13 02:26:46       1101 kopia.repository
2022-09-13 02:31:58   26005840 p024cc73dc5d7462363743bd7aac20f79-s1d843f2e82e65f72114
2022-09-13 02:29:42   21054102 p0b2cbfd1cda6bebc15bdeffa89b8a96e-s1d843f2e82e65f72114
2022-09-13 02:29:55   26767729 p124bdb974cb7fa91aa99275585329c41-s1d843f2e82e65f72114
2022-09-13 02:29:34   22964690 p15598f9c78e76b8ec91d1d1d6d35d408-s1d843f2e82e65f72114
2022-09-13 02:30:15   24047886 p173fdbd7ca15e82bcd125a4f333a263e-s1d843f2e82e65f72114
2022-09-13 02:30:20   23589167 p1f3cd580487973337860306225dccb36-s1d843f2e82e65f72114
...
```

And we can list our snapshots:
```bash
rhys@mgmt:~$ kopia snapshot list
rhys@mgmt:/home/rhys
  2022-09-13 02:29:31 UTC k1ec1fe89bba7206a48c6f3a5d7adf2ce 2 GB drwxr-xr-x files:37349 dirs:6911 (latest-1,hourly-1,daily-1,weekly-1,monthly-1,annual-1)
```

So there you have it - Kopia with ransom attack protection. Have I missed anything? 

I’ve only worked with AWS but no doubt this applies to other compatible providers. Likewise, much of what’s discussed here would apply to other backup tools. 

Huge thanks to the Kopia team for such an awesome tool.

I don’t have comments set up on this blog yet (recently migrated from WordPress) but reach out if you’ve got any corrections/questions etc. 


Ngā mihi nui  
Rhys

### References
* [https://github.com/kopia/kopia/issues/1067](https://github.com/kopia/kopia/issues/1067)
* [https://stackoverflow.com/a/52891667](https://stackoverflow.com/a/52891667)
* [https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-pagination.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-pagination.html)
* [https://docs.aws.amazon.com/AmazonS3/latest/userguide/versioning-workflows.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/versioning-workflows.html)
* [https://aws.amazon.com/premiumsupport/knowledge-center/s3-undelete-configuration/](https://aws.amazon.com/premiumsupport/knowledge-center/s3-undelete-configuration/)
