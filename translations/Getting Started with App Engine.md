# Getting Started with App Engine
# In this lab, you create and deploy a simple App Engine application
# Using a virtual environment in the Google Cloud Shell

# Objectives
- In this lab, you learn how to perform the following tasks:

- Initialize App Engine.

- Preview an App Engine application running locally in Cloud Shell.

- Deploy an App Engine application, so that others can reach it.

- Disable an App Engine application, when you no longer want it to be visible.

# 
0. Activate Google Cloud Shell
# You can list the active account name with this command:
        gcloud auth list
# You can list the project ID with this command:

gcloud config list project
# 

1. Initialize your App Engine app with your project and choose its region:
        gcloud app create --project=$DEVSHELL_PROJECT_ID


# Clone the source code repository for a sample application in the hello_world directory:
        git clone https://github.com/GoogleCloudPlatform/python-docs-samples

# Navigate to the source directory:
        cd python-docs-samples/appengine/standard_python3/hello_world

2. Run Hello World application locally
# You can run the Hello World application in a local, virtual environment in Cloud Shell.
# Ensure that you are at the Cloud Shell command prompt.

# Execute the following command to download and update the packages list.
        sudo apt-get update
# Set up a virtual environment in which you will run your application. Python virtual environments are used to isolate package installations from the system.

sudo apt-get install virtualenv -y
# 
        virtualenv -p python3 venv
# Activate the virtual environment.


source venv/bin/activate

# Navigate to your project directory and install dependencies.
        pip install  -r requirements.txt


# Run the application:
        python main.py

# To end the test, return to Cloud Shell and press Ctrl+C to abort the deployed service.

3. Deploy and run Hello World on App Engine
# To deploy your application to the App Engine Standard environment:
# Navigate to the source directory:
        cd ~/python-docs-samples/appengine/standard_python3/hello_world

# Deploy your Hello World application.
        gcloud app deploy -y

- This app deploy command uses the app.yaml file to identify project configuration.

# Launch your browser to view the app at http://YOUR_PROJECT_ID.appspot.com
        gcloud app browse

# 