---
title: "[UofTCTF'24] Out of the Bucket"
date: 2024-01-15T14:19:53+08:00
authors:
  - Kairos
tags:
  - misc
ctfs:
  - UofTCTF'24
---

## UofTCTF'24: Out of the Bucket

This was a pretty easy and interesting misc challenge that utilises Google's Cloud Storage. We are given a url to a bucket, and we have to find the flag within the bucket.

## The Challenge

```
Out of the Bucket
100
Check out my flag website!

Author: windex

https://storage.googleapis.com/out-of-the-bucket/src/index.html
```

## The Analysis

Opening the link, we are greeted with a website that looks like this:
![website](website.png)

Usually, when I get a website to look at, I would check the source code. However, this time, there was nothing in the source code.

So, I had to look deeper into this challenge. Seeing the url `storage.googleapis.com...` I found that it was a Google Cloud Storage bucket, and the text that followed was the name of the bucket.

## The Solution

I searched up tools to access files in a Bucket, and I found that there was a CLI tool called `gsutil`.

With reference to the [gsutil documentation](https://cloud.google.com/storage/docs/gsutil/commands/ls), I tried to gain more information about the bucket.

To see the files in the bucket:

```bash
kairos@opensus:~> gsutil ls gs://out-of-the-bucket
gs://out-of-the-bucket/secret/
gs://out-of-the-bucket/src/
```

Essentially, the command lists the files in the bucket, and `gs://` is used to specify that it is a Google Cloud Storage bucket. So, we see that there are two folders, `secret` and `src`. To download the files, we can use the `cp` command.

```bash
kairos@opensus:~> gsutil -m cp -r gs://out-of-the-bucket/secret Downloads/
Copying gs://out-of-the-bucket/secret/dont_show...
Copying gs://out-of-the-bucket/secret/funny.json...                             
kairos@opensus:~> 3 KiB/  2.3 KiB]  99% Done                                    
```

The `-m` flag is used to enable parallel copying (to speed up the process), and the `-r` flag is used to copy recursively. And, we get two files, `dont_show` and `funny.json`.

Looking at the contents of both files:
```bash
kairos@opensus:~> cat Downloads/secret/*
uoftctf{allUsers_is_not_safe}{
  "type": "service_account",
  "project_id": "out-of-the-bucket",
  "private_key_id": "21e0c4c5ef71d9df424d40eed4042ffc2e0af224",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDWxp...
  =\n-----END PRIVATE KEY-----\n",
  "client_email": "image-server@out-of-the-bucket.iam.gserviceaccount.com",
  "client_id": "102040203348783466577",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/image-server%40out-of-the-bucket.iam.gserviceaccount.com",
  "universe_domain": "googleapis.com"
}
```

Within `dont_show` is the flag! :D

The other file, `funny.json` is for the next part of the challenge.