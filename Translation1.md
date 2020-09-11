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

### Step 1: Deploy a web server VM instance

1. Create a **VM Instance**.
    * Name: blogpost
    * Region: us-central1
    * Zone: us-central1-a
    * Machine-Type: default
    * Boot disk image: Debian GNU/Linux 9 (stretch)
    * Identity: Debian GNU/Linux 9 (stretch)
    * API access: Debian GNU/Linux 9 (stretch)
    * Firewall: {Allow http traffic}
    
    
    **Commands**
    gcloud compute instances create blogpost \
    --zone=us-central1-a \
    firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 \
    --metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart \
    --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append \
    --tags=http-server \
    --subnet=default \
    --image=debian-9-stretch-v20200910 \
    --image-project=debian-cloud \
    --boot-disk-size=10GB \
    --boot-disk-type=pd-standard \
    --boot-disk-device-name=blogpost
    
### Step:2 Create a Cloud Storage bucket using the gsutil command line
    export LOCATION=US    
    gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
    gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
    gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
    gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
    
### Step:3 Create the Cloud SQL instance
    gcloud sql instances create blog-db --tier=db-n1-standard --region=us-central1 --database-version=MYSQL_8_0
    
    gcloud sql users set-password root --host=% --instance=blog-db --password=123456
    
### Create Network
    
    gcloud compute networks create 'web front end' \
        --region us-central1 \
        --network {external-ip-of-vm} \
    
### Step:4 Configure an application in a Compute Engine instance to use Cloud SQL

    cd /var/www/html
    sudo nano index.php
    
    In the nano text editor, replace CLOUDSQLIP with the Cloud SQL instance Public IP address that you noted above. Leave the quotation marks around the value in place.
    In the nano text editor, replace DBPASSWORD with the Cloud SQL database password that you defined above. Leave the quotation marks around the value in place.
    
    sudo service apache2 restart
    Open vm external ip in vm
    
### Step:5 Configure an application in a Compute Engine instance to use a Cloud Storage object
    Use image in step 2. {$DEVSHELL_PROJECT_ID/my-excellent-blog.png}

    cd /var/www/html
    sudo nano index.php
    
    Create image tag in html and add <img src='https://storage.googleapis.com/$DEVSHELL_PROJECT_ID/my-excellent-blog.png'>
    
    sudo service apache2 restart
    
    Check vm url to see if image is viewable