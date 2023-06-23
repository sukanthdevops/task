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
You can also customize the values of other variables, like the region.

The result should look similar to this:

region = "nyc3"
droplet_count = 3
ssh_key = "id_rsa.pub"
subdomain = "minimal-vpc"
domain_name = "example.com"
Save the changes to the file and then close it.

Step 4: Deploy the Infrastructure
Terraform requires three steps for deployment:

Initializing the directory. terraform init prepares the working directory for use by accounting for any changes in Terraform’s backend configuration.

Revewing an execution plan. terraform plan provides a detailed manifest of the resources for you to review before execution.

Applying (executing) the Terraform plan. terraform apply executes the deployment of the resources into your account.

To initialize the working directory, run the following command from the 01-minimal-web-db-stack directory:
terraform init
Next, create the Terraform plan:

terraform plan -var-file=nyc3.tfvars -out=infra.out
Copy
Terraform saves the plan to an infra.out inside the working directory. Open the file in a text editor to review your Terraform plan.

Finally, apply the plan and begin the deployment:

terraform apply "infra.out"
Copy
Terraform will create the resources outlined in the Terraform plan in your DigitalOcean account.

Step 5: View Your Infrastructure
You can view the resources being created in the control panel, like the Droplets, database, and load balancer.

Once the deployment is complete, you have deployed the infrastructure. To view the website being hosted by the Droplets, open a web browser and enter minimal-vpc.example.com, replacing example.com with the domain you used in the deployment.

Click your browser’s Reload or Refresh button to see how the load balancer is distributing traffic across the Droplets using a round-robin algorithm.

Step 6: Destroy Resources (Optional)
You can delete resources from your account using Terraform with one command. You can destroy specific resources or all the resources deployed using your .tfvars file.

To destroy all of the resources you created during this tutorial, run:

terraform destroy -var-file=nyc3.tfvars
Copy
Terraform will ask you to confirm the deletion.

Summary
Once you’ve deployed resources into your account, you can use this same workflow to deploy similar architectures for other web applications into your account. You only need to complete these steps once:

Create an API token.
Download Terraform.
Set up environmental variables.
To deploy additional architectures using the same configuration, run the following commands from the Terraform directory:

Run terraform init to initialize the directory.
Run terraform plan -var-file=nyc3.tfvars -out=infra.out to create and review your Terraform plan.
Review your Terraform plan by opening infra.out file in a text editor.
Run terraform apply "infra.out" to deploy resources.
What’s Next?
After deploying the web application infrastructure, you can use your new Droplets and network to host your websites and applications.

You can also edit the Terraform files to experiment and make changes to the infrastructure. Use our reference documentation to learn about more options:
