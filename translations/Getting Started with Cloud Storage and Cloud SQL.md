# Getting Started with Cloud Storage and Cloud SQL
- In this lab, you create a Cloud Storage bucket and place an image in it. 
- You will also configure an application running in Compute Engine to use a database managed by Clou SQL.
- You will configure a web server with PHP, a web development environment that is the basis for popular blogging software. 
- You also configure the web server to reference the image in the Cloud Storage bucket.


# Objectives
- Create a Cloud Storage bucket and place an image into it.

- Create a Cloud SQL instance and configure it.

- Connect to the Cloud SQL instance from a web server.

- Use the image in the Cloud Storage bucket on a web page.

1. Deploy a web server VM instance
# We need to create a web server VM Instance
# Launch the Cloud Shell Interface and execute
        gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --tags=http-server
# Firewall*
        gcloud compute firewall-rules create default-allow-http --direction=INGRESS --action=ALLOW --rules=tcp:80 --target-tags=http-server
# Startup Script
        apt-get update
        apt-get install apache2 php php-mysql -y
        service apache2 restart
# take note of VM internal and external IP address

# 2. Create a Cloud Storage bucket using the gsutil command line
# For convenience, enter your chosen location into an environment variable called LOCATION. Enter one of these commands:
        export LOCATION=US

# In Cloud Shell, the DEVSHELL_PROJECT_ID environment variable contains your project ID. Enter this command to make a bucket named after your project ID:
        gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID

# Retrieve a banner image from a publicly accessible Cloud Storage location:
        gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png

# Copy the banner image to your newly created Cloud Storage bucket:
        gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

# Modify the Access Control List of the object you just created so that it is readable by everyone:
        gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

# 