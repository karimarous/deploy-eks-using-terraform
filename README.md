# Payever DevOps Task

## Overview

This repository contains the mainfest of the trial task given by Payever team.

## Details

The root repository contains the following files and folders.

```bash
├── aws
├── es-to-prom-exporter
├── mysql
├── monitoring
├── logging
├── namespaces.yaml
└── README.md
```

### Files and Folder Details




#### 1. aws

It contains the manifest regarding the cluster infrastructure. Detials of this folder is given below:

```bash
├── eks # it contains the mainfest for the infrastucture
│   ├── kubeconfig_eks-cluster # it is generated by terraform
│   ├── main.tf # it contains the information regarding the eks cluster infrastructure and user permissions
│   ├── terraform.tfstate # it contains the state of the infrastructure generated by terraform using the main.tf file
│   └── terraform.tfstate.backup # it contains the terraform's state backup
└── users # it contains the manifest for user and related resource creation
    ├── eks-user-policy.json # it contains the required permission for the user to create an eks cluster
    ├── main.tf # it contains the information regarding the user creation
    ├── terraform.tfstate # it contains the state of the user and related resources created by terraform
    └── terraform.tfstate.backup # it contains the backup of terraform's state
```

##### Deployment

1. For the deployment of the above resource we need following tools/utilities

    1. AWS account and a user configured.
    
    2. terraform (version: v0.12.21(for user creation), v0.11.14(for eks cluster creation)). Two version are required due to support for the manifests. This can be resolved by some research.

    3. aws-iam-autheticator.

    4. kubectl

2. Move inside the `users` folder and run the following command:

```bash
terraform apply # it will output the plan for resources creation and ask for permission to create the resources.

# it wil create two files regarding the terraform state. Once user is created it ACCESS_KEY and ACCESS_KEY_SECRET needs to be configured on the system in order to create the eks cluster. These secret can be found either in the terraform.tfstate file or AWS IAM console.
```

It will create a user in AWS with limited access.

3. Move inside the `eks` folder and run the following command:

```bash
terraform apply # it will output the plan for resources creation and ask for permission to create the resources.

# it will create three files, two of them contains the terraform state while another files contains the eks cluster kubeconfig. In the above apply command 1 step might fail. It can be resolved by adding the kubeconfig in this file on your system `~/.kube/config`. Once added re-run the above command. 
```

4. Once above steps are completed we will have a user with limited access to create an eks cluster and a runnnig eks cluster.

5. To verify the cluster up use the command given below:
```bash
kubectl get nodes # it will output the cluster nodes based on the configuration provied in eks/main.tf file.
```

### 2. mysql

It contains the manifest for mysql. Detials of this folder is given below:

```bash
├── mysql.yaml # it contains the manifest for the mysql
└── secrets
    └── mysql-passwords.yaml # it contains the configuration for the mysql database. It will be used in mysql.yaml file.
```



### 3. monitoring
### 4. logging
### 5. namespace.yaml






deploy dashboard

https://docs.aws.amazon.com/eks/latest/userguide/dashboard-tutorial.html

https://github.com/kubernetes-sigs/metrics-server/releases/tag/v0.2.1