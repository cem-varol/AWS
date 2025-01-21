# AWS
 CI/CD implementation  
 
   1- Use GitHub as VCS. Codecommit can be used instead of GitHub. Since CodeCommit can't be used anymore GitHub choosen
   2- A Python app created and pushed into GitHub
   3- Go to "CodeBuild" service section from AWS Console
   4- Click "Create project". Project Name:  <python-app-build>
   5- Select "Source" from drop-down <github>. Choose "Public Repository" option. Enter <cem-varol/AWS/blob/main/CI-CD>
   6- Do relevant settings for GitHub connection. Provide PAT.
   7- Choose "On-Demand" "Managed image" "EC2", "OS" options from "Environment" section
   8- CodeBuild should use a Role for to access this EC2 instance. Select "New service role" option and give the name <codebuild-python-app-build-service-role>
   9- Write the yaml definiton of BuildSpec file
      - Click "Switch to editor"
      - 
   

   
 
