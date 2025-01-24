# AWS Lambda Stale Resource Elemination

This project is amied to find stale resources in AWS Cloud Infrastructure. 
In the example scenario an instance was created by IAM user and after a while 
a snapshot was taken by this user. Later then the instance was deleted but snapshot 
forgotton. According to this scenario an event triggered based lamda function is created 
to determine such zombie resources.


Steps: 

EC2: 
- Create an EC2 instance
- Take a snapshot of the volume belongs to this EC2 instance

Lambda: 
- Create a lambda function 
- Upload the source code 
- Test the code 
- It will be failed about execution duration( 3 seconds). IN configuration time incerease the execution time to 10 seconds.
- Test again. 
- Soruce code will fail again because of permissions. 
  - Code checks the exsitance instances. If therre is an instance actively running code does nothing.
    If there is a snapshot that belongs to instance and the instance exists no more, this zombie snapshot 
    will be deleted by code. For this reason we should give following permission to lambda function: 
    1- DecribeInstances
    2- DescribeSnapshots, DeleteSnapshosts

Lambda Function Permission Settings:

- Navigate configuration tab, select permission section
- Click the execution role 
- In new window click attach policies.
- If you can not find the policy here, navigate to policies in IAM
- Click create policy 
- Choose the service as EC2
   - find the policy DescribeInstances and select it
   - click next and give a proper name
   - click create policy 
   
- Turn back to lambda function permissions window, click again add permisssions
- Select attach policies 
- search your newly created policy  "aws-opt-cost-desc-instances"
- In the list select to policyi and than click add permission

Now our lambda function has permission to query instances
DO SAME PROCEDURE fOR EBS VOLUME PERMISSIONS.

Create an EC2 Instance

- take a snapshot of the volume of the instance
- Delete the instance
- make sure volume also deleted
- Deploy and triggger the Lambda function 
- Check the status of the zombie snapshot. 
IT SHOLUD BE DELETED BY LAMBDA FUNCTION 

BEST PRACTICE:
- Configure this lambda function to be triggered from CloudWatch daily basis.

   
