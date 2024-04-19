# Python-based-Client-Server-Application
 Python-based two-tier application using AWS EKS and ensure it's running reliably and securely on the Kubernetes cluster.
 [Flask App with DB-MySQL]

## To run this two-tier application in EKS Cluster 

- Create namespace "app-network" before applying manifests.

#### Pre-requisites: 
  - an EC2 Instance (Note : If Using Ubuntu EC2 Instance instead of Amazon Linux then Make Sure to have **aws-iam-authenticator** installed.)


#### Article to Install aws-iam-authenticator :
```sh
https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
```

#### AWS EKS Setup 

### IAM Setup

1. **Create an IAM User:**
   - Go to the AWS IAM console.
   - Create a new IAM user named "eks-admin."
   - Attach the "AdministratorAccess" policy to this user.

2. **Create Security Credentials:**
   - After creating the user, generate an Access Key and Secret Access Key for this user. You will need these credentials later.

### EC2 Instance Setup

3. **Launch an EC2 Instance:**
   - Choose the desired region (e.g., us-west-2).
   - Launch an Ubuntu instance. Make sure to configure the security group to allow SSH access.

4. **SSH to the EC2 Instance:**
   - Use your local terminal to SSH into the instance:
     ```
     ssh -i <path-to-your-key-file> ubuntu@<instance-public-ip>
     ```

5. **Install AWS CLI v2:**
   - Download and install the AWS CLI v2:
     ```
     curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     sudo apt install unzip
     unzip awscliv2.zip
     sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
     ```

6. **Configure AWS CLI:**
   - Configure the AWS CLI with the Access Key and Secret Access Key from step 2:
     ```
     aws configure
     ```

### Kubernetes Tools Setup

7. **Install kubectl:**
   - Download and install kubectl:
     ```
     curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
     chmod +x ./kubectl
     sudo mv ./kubectl /usr/local/bin
     kubectl version --short --client
     ```

8. **Install eksctl:**
   - Download and install eksctl:
     ```
     curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
     sudo mv /tmp/eksctl /usr/local/bin
     eksctl version
     ```

### EKS Cluster Setup

9. **Create an EKS Cluster:**
   - Use eksctl to create the EKS cluster (replace `<cluster-name>` and `<region>` with your desired values):
     ```
     eksctl create cluster --name <cluster-name> --region <region> --node-type t2.micro --nodes-min 2 --nodes-max 2
     ```

10. **Update Kubeconfig:**
    - Update your kubeconfig to connect to the newly created EKS cluster:
      ```
      aws eks update-kubeconfig --region <region> --name <cluster-name>
      ```

11. **Verify Nodes:**
    - Verify that the EKS nodes are running:
      ```
      kubectl get nodes
      ```

### Deploying and Managing Applications

12. **Run Manifests:**
    - Create a Kubernetes namespace (replace `<namespace>` with your desired namespace name):
      ```
      kubectl create namespace <namespace>
      ```
    - Apply your Kubernetes manifests (replace `<path-to-manifests>` with the path to your manifests):
      ```
      kubectl apply -f <path-to-manifests>
      ```

13. **Delete Manifests:**
    - If needed, delete the deployed resources:
      ```
      kubectl delete -f <path-to-manifests>
      ```

### Cleaning Up

14. **Delete EKS Cluster:**
    - When you're done with your EKS cluster, you can delete it:
      ```
      eksctl delete cluster --name <cluster-name> --region <region>
      ```

This guide should help you set up and manage an AWS EKS cluster step by step. Make sure to replace `<cluster-name>`, `<region>`, `<namespace>`, and `<path-to-manifests>` with your specific values and paths as needed.


