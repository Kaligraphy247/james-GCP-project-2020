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

# 1. Deploy a web server VM instance
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

# 3. Create the Cloud SQL instance

# 4 Configure an application in a Compute Engine instance to use Cloud SQL
# Connect to VM_NAME via SSH
# To connect to VM_NAME via SSH, execute
        gcloud compute ssh VM_NAME
# In your ssh session on bloghost, change your working directory to the document root of the web server:
        cd /var/www/html
# Use the nano text editor to edit a file called index.php:
        sudo nano index.php
# Paste the content below into the file:
        <html>
        <head><title>Welcome to my excellent blog</title></head>
        <body>
        <h1>Welcome to my excellent blog</h1>
        <?php
        $dbserver = "CLOUDSQLIP";
        $dbuser = "blogdbuser";
        $dbpassword = "DBPASSWORD";
        // In a production blog, we would not store the MySQL
        // password in the document root. Instead, we would store it in a
        // configuration file elsewhere on the web server VM instance.

        $conn = new mysqli($dbserver, $dbuser, $dbpassword);

        if (mysqli_connect_error()) {
        echo ("Database connection failed: " . mysqli_connect_error());
        } else {
                echo ("Database connection succeeded.");
        }
        ?>
        </body></html>
# Press Ctrl+O, and then press Enter to save your edited file.
# Press Ctrl+X to exit the nano text editor.

# Restart the web server:
        sudo service apache2 restart
# Open a new web browser tab and paste into the address bar your bloghost VM instance's external IP address followed by /index.php. The URL will look like this:
        35.192.208.2/index.php
# When you load the page, you will see that its content includes an error message beginning with the words:
        Database connection failed: ...

- This message occurs because you have not yet configured PHP's connection to your Cloud SQL instance.
# Return to your ssh session on bloghost. Use the nano text editor to edit index.php again.
        sudo nano index.php
# In the nano text editor, replace CLOUDSQLIP with the Cloud SQL instance Public IP address that you noted above. Leave the quotation marks around the value in place.

# In the nano text editor, replace DBPASSWORD with the Cloud SQL database password that you defined above. Leave the quotation marks around the value in place.

# Press Ctrl+O, and then press Enter to save your edited file.

# Press Ctrl+X to exit the nano text editor.

# Restart the web server:
        sudo service apache2 restart
# Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, the following message appears:
        Database connection succeeded
# 5. Configure an application in a Compute Engine instance to use a Cloud Storage object
# Return to your ssh session on your bloghost VM instance.

# Enter this command to set your working directory to the document root of the web server:
        cd /var/www/html
# Use the nano text editor to edit index.php:
        sudo nano index.php
- Use the arrow keys to move the cursor to the line that contains the h1 element. Press Enter to open up a new, blank screen line, and then paste the URL you copied earlier into the line.

# Paste this HTML markup immediately before the URL:
        <img src='
# Place a closing single quotation mark and a closing angle bracket at the end of the URL:
        '>
# Press Ctrl+O, and then press Enter to save your edited file.

# Press Ctrl+X to exit the nano text editor.

# Restart the web server:
        sudo service apache2 restart