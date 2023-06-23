# flux-eks
GitOps synchronising GitHub repo with EKS cluster on AWS

Installing Flux on EKS For Simplified Kubernetes Deployment Automation 
Discover the simple steps to set up Flux on Amazon EKS and automate your Kubernetes deployments with ease
picture here

Introduction: 
Automating Kubernetes deployments can greatly enhance the efficiency and reliability of your software delivery process. In this tutorial, we will walk through the steps to install Flux, a powerful GitOps tool, on Amazon Elastic Kubernetes Service (EKS). Flux enables you to automate the deployment of Kubernetes manifests, making it easier to manage and scale your applications. Let's dive in!

Prerequisites: 
Before we begin, make sure you have the following prerequisites in place:

1. Homebrew Package Manager:
If you don't have Homebrew installed, you can install it by running the following command in the terminal (MacOs):

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
Alternatively click here to find the appropriate install instructions (https://brew.sh/)
2. eksctl: Install eksctl on your system using Homebrew by running the command:
brew install eksctl
3. Have Git installed:
brew install git

Step 1: Create EKS Cluster
Create an EKS cluster using "eksctl". If you haven't done this already, you can use the following command to create a cluster (replace <cluster_name> with your desired cluster name):
eksctl create cluster <cluster_name>
Verify Cluster creation in the terminal by running the following command:
eksctl get cluster
Head to your AWS console and verify the cluster creation in the EKS console.

Step 2: Install Flux
Now Install Flux next, let's install Flux on your local system using Homebrew. Run the following command:
brew install fluxcd/tap/flux

Step 3: Create A New GitHub Repository:
Go to GitHub (https://github.com) and create a new repository to store your Kubernetes manifests. Take note of the repository URL as we will need it later.

Step 4: Create A GitHub Token:
Visit the GitHub settings page (https://github.com/settings/tokens) and generate a new personal access token. Provide a descriptive name for the token and enable the repo scope. Click on "Generate token" and make a note of the generated token. We will use it for authentication in the later steps.

Step 5: create an SSH Key in GitHib:
Open your terminal and run the following command to generate an SSH key pair:
ssh-keygen -t ed25519 -C "your-email@example.com"
Press enter to accept the default location and set a passphrase if desired. Copy the public key to your clipboard with the following command:
pbcopy < ~/.ssh/id_ed25519.pub
Visit the SSH and GPG keys settings page on GitHub (https://github.com/settings/keys). Click on "New SSH Key" and paste the copied key into the "Key" field.
Provide a descriptive title for the key and click on "Add SSH Key" to save it.

Step 6: Export GitHub Token and Username:
 Export your GitHub personal access token and username by running the following commands in the terminal:
export GITHUB_TOKEN=<your-token>
export GITHUB_USER=<your-username>

Step 7: Bootstrap Flux:
On EKS Before bootstrapping Flux, let's perform a pre-check to ensure everything is in order. Execute the following command:
flux check -pre
Once the pre-check passes, we can proceed to bootstrap Flux by running the following command:
flux bootstrap github \
  --components-extra=image-reflector-controller,image-automation-controller \
  --owner=<github_user_name> \
  --repository=<repository_name> \
  --branch=main \
  --path=./clusters/eks \
  --token-auth \
  --personal
  
Replace <github_user_name> with your GitHub username and <repository_name> with the name of the repository you created earlier.

Step 6: Verify Flux Installation:
Finally, head over to your GitHub Flux repository and verify the successful installation. You should see Flux syncing with your repository and automatically applying any changes made to your Kubernetes manifests.

This command checks to see if you have everything needed to run Flux:
flux check - pre
flux bootstrap github \
 - components-extra=image-reflector-controller,image-automation-controller \
 - owner=<github_user_name> \
 - repository=<repository_name> \
 - branch=main \
 - path=./clusters/eks \
 - token-auth \
 - personal
Head over to you GitHub flux repository and verify the successful installation.
