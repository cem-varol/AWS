# AWS
 CI/CD implementation  
 
- Use GitHub as VCS. Codecommit can be used instead of GitHub. Since CodeCommit can't be used anymore GitHub choosen
- A Python app created and pushed into GitHub
- Go to "CodeBuild" service section from AWS Console
- Click "Create project". Project Name:  <python-app-build>
- Select "Source" from drop-down <github>. Choose "Public Repository" option. Enter <cem-varol/AWS/blob/main/CI-CD>
- Do relevant settings for GitHub connection. Provide PAT.
- Choose "On-Demand" "Managed image" "EC2", "OS" options from "Environment" section
- CodeBuild should use a Role for to access this EC2 instance. Select "New service role" option and give the name <codebuild-python-app-build-service-role>
   9- Write the yaml definiton of BuildSpec file
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
10- Use AWS System Manager to store Docker Hub credentials
        - go to "Parameter Store"
        - add: 	
	       /python-app/doker-credentials/username
                	
		/python-app/doker-credentials/password
	
		/python-app/doker-credentials/url


11- Click "create project"

12- Click "Start Build" and observe the results. 
   
 
