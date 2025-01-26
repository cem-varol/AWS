# AWS ECR-ECS APPLICATION DEPLOYMENT
This project is amied to create a repo on AWS Elastic Container Registry Service. 
By using this registry we will deploy python based  container image to AWS Elastic Container Service 
cluster. Steps are given below in detail:  

Steps: 

Create a container registry in ECR

- Navigate through to ECR service from AWS Console
- Click to "Create" button
- Choose private registry. Because by nature private registry is the aim of use of ECR service
- In general settings give a namespace and repo name: /python-app
- Set the other options as is 
- Click "create button"
- Now you can see your new repo in the list 

Create a Docker image of your application.

- Navigate through the root folder of python-app from terminal
- Run following command 
    docker build -t python-app .
- tag your image as decribed below. Command is taken from AWS "view push commands" pop-up window in ECR service
    docker tag python-app:latest 381492062297.dkr.ecr.us-east-1.amazonaws.com/python-app:latest 


Push docker image of the python app to ECR 

- Click on the "python-app" repo from the repository list
- In Images section there is no images yet 
- Click	 View Push Commands" button  
- Push the image to ECR
   docker push 381492062297.dkr.ecr.us-east-1.amazonaws.com/python-app:latest
- Now you image is listed in under images section of ECS


Create AWS Elastic Contianer Service(ECR) cluster: 

- Navigate through to ECR service from AWS Console
- Click on "clusters" from menu on the right
- Click "Create Cluster" button 
- Give a name to your cluster: "python-app-cluster"
- Choose Infrastructure as Fargate. Fargate is a serverless solution for ECS. 
- Click create 


Create Task Definition of your image: 
- Click on the "python-app-cluster" from the cluster list
- Click on  the "Task Definition " from the menu on right
- Click on "Create New Task Definition" button
- Provide "task definition family name": python-app-task-def
- Choose "Launch Type" as "AWS Fargate"  
- Choose OS System as "Linux/X86_64"
- Choose CPU: 1	vCPU, 2GB Memory
- Choose "Taks Role" as: None
- Choose "Task Execution Role" : create a new role 
- Provide "Container Name": PythonApp
- Provide repo url and image tag: 381492062297.dkr.ecr.us-east-1.amazonaws.com/python-app:latest
- Set container port: 3000
- Provide port name: pythonApp-3000-tcp
- Keep others as is
- Integrate it with CloudWatch 
- Click "Create"

Run Task: 
- We created "task definition" previously. Now we will run the "task". (Task can be considered as container in ECS)
- Click "Task Ddefinitions" from right menu
- Click on the name of task listed
- Click on "Deploy" button and choose "Run Task
- In new window choose cluster as: "python-app-cluster"
- Clcik on the "create" button
- Verify your task(container) is running in ECS cluster.Navigate to cluster and see it under "Tasks" tab 
