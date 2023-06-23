# Deploy A Sample Web Application Using Terraform
<img width="575" alt="sample web application Architecture" src="https://github.com/sukanthdevops/task/assets/111341727/770f79fe-afa4-4101-afa9-ecfdf0942780">
  # Prerequistites
    Before starting this tutorial, you need four things:
    You need to create an API token in your DigitalOcean account. This allows Terraform to access the DigitalOcean API and programmatically deploy resources into your account.

You need to own a domain name and have its DNS records hosted by DigitalOcean. The domain name allows you to access the newly deployed Droplets via a hostname.

You need to have an SSH key and upload your public key to your DigitalOcean account. This is the SSH key that Terraform applies to the Droplets being deployed.

You need to install Terraform on the machine you are deploying from. Terraform is available for MacOS, Windows, Linux, and FreeBSD.
Step 1: Download the DigitalOcean Sample Terraform Repo
We created a GitHub repository with several Terraform configuration files that compose the web architecture. You need to download this repository to your machine to deploy the resources. This does not require a GitHub account.

First, download the repository to your system:

git clone https://github.com/do-community/terraform-sample-digitalocean-architectures
Once you download the repository, move to the 01-minimal-web-db-stack directory:

cd terraform-sample-digitalocean-architectures/01-minimal-web-db-stack
Copy
This directory is where you execute Terraform commands from.

Step 2: Configure Terraform Environment Variables
Before running Terraform commands, you need to assign your API token as an environment variable. This allows Terraform to access your DigitalOcean account via the API.
Environment variables are stored on your operating system outside of your working directory. This can be useful for storing variable values that are consistent across your DigitalOcean Terraform deployments, such as your API token or datacenter regions.

To assign your API token as an environment variable, run the following command, entering your API token and the path to your private SSH when prompted:

echo Enter your DigitalOcean API token?; read token; export TF_VAR_do_token=$token
Copy
The command returns a blank prompt when successful. To verify the variable was created, print the value of the variable to the command line:

echo $TF_VAR_do_token
The variable is set correctly if the command prints your key.

Step 3: Assign Values to Input Variables
Next, you need to customize the architecture to match your needs, like setting your domain name and SSH key. To do this, you assign values to certain Terraform variables in its configuration files.
In the sample architecture, we assigned values for several required variables in the nyc3.tfvars file which you need to customize before deployment.

To edit the input variables, open the nyc3.tfvars file in a text editor:

nano nyc3.tfvars

Then change the values of the ssh_key and domain_name fields.

For the ssh_key field, enter the file name of an SSH key you have previously uploaded to your account.
In the domain field, enter a domain name hosted on your DigitalOcean account.
