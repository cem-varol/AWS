AWS TERRAFROM RESOURCE PROVISIONING

This project implmeneted as to apply IaC concepts with Terraform.
Following resources will be provisioned to AWS by using Terraform

    - VPC, two public subnets in different AZ, one internet GW, route table and two route table associations. 
    - Two EC2 insntances - One S3 Bucket - One Loadbalancer and target group - One Listener 
    - Two target group attachment
    - In two EC2 instances there will be a webapp. This webapps are loaded to EC2 instances with initial scripts during startup.
    - These two instances are attached to different public subnets in diffrenet AZ. 
    - Both Subnets are ended with an S3 bucket.

Steps:

	1- Setup Hashicorp Terraform in local computer.
	2- Install AWS CLI 
	3- Create provider.tf file. Take code snippet from Terraform docs.
	4- Create main.tf file. Write your code inside this.
	4- Navigate inside the project folder. Initialize the project
	 terrraform init 
	5- To validate the code, run following command
               > terraform validate
	6- Then inorder to see whay will happen after provisioning run
               > terrform plan 
	7- If everything seems ok run 
               > terraform apply
	8- Now all your resources are provisioned.
	9- To delete all resources run
    	      > terrafrom destroy 

