# Getting Started with Compute Engine
# In this lab, we shall create virtual machines and connect to them
# We will aslo create connections between the instances

1. Create a virtual machine using the GCP Console 
Open the Cloud Shell Interface and type in the following (you could copy and paste)
# each command should be followed by "Enter" on your keyboard  

    gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --tags=http-server

# this creates a VM with the name "my-vm-1" 
# in the region  "us-central-1" 
# and zone "a*"
# with the default machine type "n1-standard-1" and image fle "debian-9-stretch-v20200902"
# and image project "debian-cloud" with a boot disk size of "10GB"

    gcloud compute firewall-rules create default-allow-http --direction=INGRESS --action=ALLOW --rules=http:80 --target-tags=http-server
# this creates a firewall rule that allow http traffic in the Virtual Machine 

2. Create a virtual machine using the gcloud command line
# use this partial command to list all the zones in the region you want to run your VM
    gcloud compute zones list | grep us-central-1
# set you zone to "us-central1-b"
    gcloud config set compute/zone us-central1-b

# use this to create a VM instance called "my-vm-2" in the zone above
    gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

3. Connect between VM instances
    3.1 Use the "ping" command to confirm that my-vm-2 can reach my-vm-1 over the network
        gcloud compute ssh my-vm-2
# this connects to my-vm-2 via ssh
   
        ping -c 3 my-vm-1
# this will attempt to ping my-vm-1 3 times 
    3.2 Install nginx web server on my-vm-1
# use the ssh command line to open a terminal on  my-vm-1 from my-vm-2
        ssh my-vm-1
# install nginx web server
        sudo apt-get install nginx-light -y
# Use the nano text editor to add a custom message to the home page of the web server
        sudo nano /var/www/html/index.nginx-debian.html
# Use the arrow keys to move the cursor to the line just below the h1 header. 
# Add text like this, and replace YOUR_NAME with your name 
        Hi from James
# Press CTRL + O and the press "Enter" to save 
# press CTRL + X to exit the nano text editor
    3.3 Confirm that the web server is running
# on your COmmand line on my-vm-1, execute:
        curl http://localhost/
# now, exit my-vm-1 command line
        exit
# to confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:
        curl http://my-vm-1/
    3.4 Get the external IP address for my-vm-1
# to get the external IP address for my-vm-1, execute this command
        gcloud compute instances list
# copy the IP address for my-vm-1, open a new tab on your browser and paste and go
# you will see your web server's homepage including your custom text 