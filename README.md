# AWS S3 Static Website Hosting with CloudFront OAC

This repository contains the Terraform configuration to securely deploy a high-availability static website on AWS. The infrastructure serves content out of an Amazon S3 bucket via an Amazon CloudFront Distribution utilizing **Origin Access Control (OAC)** to ensure the S3 bucket remains entirely private from the public internet.

---

## Architecture Overview

Below is the structural outline of the deployment. Public users can only access the website assets via the HTTPS CloudFront Edge locations. CloudFront securely authenticates against the private S3 bucket using AWS Signature Version 4 (SigV4).

                   +-------------------+
                   |    Public User    |
                   +---------+---------+
                             |
                             | HTTPS (Port 443)
                             v
            +---------------------------------+
            |  Amazon CloudFront Distribution |
            +----------------+----------------+
                             |
                             | OAC (SigV4 Authentication)
                             v
          +-------------------------------------+
          | Amazon S3 Bucket (Private Access)   |
          |                                     |
          |  [ /index.html ]   [ /css ]  [...]  |
          +-------------------------------------+

## Infrastructure Components

* **Amazon S3**: Stores the static website assets (HTML, CSS, JS, Images). Public access blocks are enabled to prevent accidental exposure.
* **Amazon CloudFront**: Global Content Delivery Network (CDN) caching assets at edge locations for low-latency delivery. Forces communication over HTTPS.
* **Origin Access Control (OAC)**: The modern cryptographic signing security method that allows CloudFront to securely pull assets from the private S3 bucket.
* **Automated Content Upload**: Utilizes dynamic key mapping and `filemd5` tracking to automatically upload and sync contents from your local `./www` directory with correct MIME types.

---

## File Structure

```text
├── main.tf                 # Primary Terraform infrastructure definitions
├── README.md               # Project documentation
└── www/                    # Local directory containing your static website files
    ├── index.html          # Application entrypoint
    ├── css/
    └── js/


          
