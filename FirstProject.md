# Associate Cloud Engineer: Learning Phase 1 Main Track Channel (2020)

## Course: Google Cloud Platform Fundamentals - Core Infrastructure

## Module: Storage in the Cloud

### LAB: GCP Fundamentals: Getting Started with Cloud Storage and Cloud SQL

### Objectives

In this lab, you learn how to perform the following tasks:

* Create a Cloud Storage bucket and place an image into it.
  
* Create a Cloud SQL instance and configure it.
  
* Connect to the Cloud SQL instance from a web server.
  
* Use the image in the Cloud Storage bucket on a web page.

### Steps

1. Create a **VM Instance**.
    * Name: blogpost
    * Region: {region-name}
    * Zone: {zone-name}
    * Machine-Type: {default}
    * Boot disk image: Debian GNU/Linux 9 (stretch)
    * Identity: Debian GNU/Linux 9 (stretch)
    * API access: Debian GNU/Linux 9 (stretch)
    * Firewall: {Allow http traffic}
    
    
    **Commands**
    gcloud beta compute --project=qwiklabs-gcp-01-4c131b616753 instances create blogpost --zone=us-central1-a --machine-type=e2-medium --subnet=default --network-tier=PREMIUM --metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart --maintenance-policy=MIGRATE --service-account=323988335651-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=blogpost --reservation-affinity=any
    
    gcloud compute --project=qwiklabs-gcp-01-4c131b616753 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
