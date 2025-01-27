# AWScodedeploy.amazonaws.com
 CI/CD implementation  

 CI PART-I: CREATE A CODEBUILD PROJECT FOR BULDING AND CREATING DOCKER IMAGE
 
- Use GitHub as VCS. Codecommit can be used instead of GitHub. Since CodeCommit can't be used anymore GitHub choosen
- A Python app created and pushed into GitHub
- Go to "CodeBuild" service section from AWS Console
- Click "Create project". Project Name:  "python-app-build"
- Select "Source" from drop-down as "github". Choose "Public Repository" option. Enter <cem-varol/AWS/blob/main/CI-CD>
- Do relevant settings for GitHub connection. Provide PAT.
- Choose "On-Demand" "Managed image" "EC2", "OS" options from "Environment" section
- CodeBuild should use a Role for to access this EC2 instance. Select "New service role" option and give the name "codebuild-python-app-build-service-role"
- Write the yaml definiton of BuildSpec file
- Click "Switch to editor"
- insert following build yaml:

--------------------------------------------------------------------------------------------------------------
            version: 0.2
            env:
             parameter-store:
               DOCKER_REGISTRY_USERNAME: /python-app/docker-credentials/username
               DOCKER_REGISTRY_PASSWORD: /python-app/docker-credentials/password
               DOCKER_REGISTRY_URL: /python-app/docker-credentials/url
             phases:
             install:
              runtime-versions:
                python: 3.11
             pre_build:
              commands:
                - echo "Installing dependencies..."
                - pip install -r python-app/requirements.txt
              build:
              commands:
                 - echo "Running tests..."
                 - cd python-app/
                 - echo "Building Docker image..."
                 - echo "$DOCKER_REGISTRY_PASSWORD" | docker login -u "$DOCKER_REGISTRY_USERNAME" --password-stdin "$DOCKER_REGISTRY_URL"
                 - docker build -t "$DOCKER_REGISTRY_URL/$DOCKER_REGISTRY_USERNAME/python-app:latest" .
                 - docker push "$DOCKER_REGISTRY_URL/$DOCKER_REGISTRY_USERNAME/python-app:latest"
              post_build:
              commands:
                 - echo "Build completed successfully!"



   --------------------------------------------------------------------------------------------------------
- Use AWS System Manager to store Docker Hub credentials
        - go to "Parameter Store"
        - add: 	
	       /python-app/doker-credentials/username
                	
		/python-app/doker-credentials/password
	
		/python-app/doker-credentials/url

- Click "create project"

- Click "Start Build" and observe the results. 


 CI PART-II: CREATE A PIPELİNE AS ORCHESTRATOR  
 INTEGRATE WITH CodeBuild:

 - Go to CodePipeline page from AWS Console
 - Give pipeline name as "python-app-pipeline"
 - Check "Allow AWS CodePipeline to create a service role.." checkbox
 - Click Next and choose GitHub. Do all GitHub conncetion settings. Click Next
 - Choose Build Provider as "AWS CodeBuild". Choose peroject as "python-app-code-build"
 - Choose "single build"
 - Skip other stages and click "Create Pipeline"
 - Now Github, CodePipeline and CodeBuild integration was made
 - Commit a new source code to git repo. This new code code commit should triggger entire build process
 - We are done !!

 

 CD PART: CREATE A CODEDEPLOY PROJECT AND INTEGRATE WITH CodePipeline:

 - Go to CodeDeploy section in AWS Console
 - Click "Create Application". Give app name as "python-app-deploy"
 - Choose compute platform as "EC2/On-premises"
 - Click on create application
 - Create an EC2 instance as  deployment target
 - Give a tag to EC2 instance for CodePipeline to identify this machine
 - Install CodeDeploy agent to EC2 instance.
      - Login to EC2 instance
      - Go documentation for CodeDeploy agent installation in AWS website. Apply all steps
 - Give permission to EC2 instance for to talk CodeDeploy. Create a service role and attach the policy
      - Go to IAM. Click Roles
      - Click create a new role
      - Select CodeDeploy from service list for permission. click next
      - Give the role name and then click "Create Role"
 - Assign this role to EC2 instance.Go to EC2 instance. "Select actions" Choose "Security" and "Modify IAM Role"
 - Restart the agent service
 - In CodeDeploy define created EC2 instance as target. Click "Create Deployment Group"
 - Enter "Deployment Group Name" as "python-app-deployment"
 - Use the same service role previously created.(Before this give EC2FullAcces policy to this service role)
 - In "Environment Configuration" click "Amazon EC2" checkbox
 - Enter the machine tag name | deployment-machine. Watch for "1 unique matched instance" warning
 - Uncheck "Enable LoadBalancing" and "Create Deployment Group"
 - Now click "Create Deployment"
 - Choose deployment group "python-app-deployment"
 - Choose "My application is on GitHub" option
 - Give commit ID and click "Create Deployment"
 - IMPORTANT: put apspec.yaml in the root of the repository.
