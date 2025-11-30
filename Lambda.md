Lambda 

It is a compute service. 
It is serverless service , where you do not need to manage server, server exists but you do nt need to manage it, hence the name serverless. You just give code to it and its runs it.
You give conditions at which a code is to be executed to code is to be triggered then Lambda can be used. These are basic use cases.
Pay model: Pay per run, means if it runs for 200 ms then you will need to pay for only 200ms only. (Another example for serverless is DynamoDB)
At the backend the service and code we give runs on very light weight machine using firefly which is aws internal technology.

AWS lambda logs can be monitored very simply using cloudwatch

🔹 Question 1: What is AWS Lambda?
-> Serverless compute service. Means we do not need to manage the server in which code is running , AWS does that. The RAM, CPU management is managed by AWS.

🔹 Question 2: How does AWS Lambda work?
-> It works on allowing us to upload the application code and then lamdha automatically takes care of infra for code to run. It can be triggeed by various AWS services, HTTP requests or scheduled events.(You see 'Add trigger' otion seen on console)

🔹 Question 3: What is the maximum execution time for an AWS Lambda function?
-> It is 15 mins. Default is 3 seconds(the timeout you see), so our job should end in 3 seconds. But we can increase it upto 15 monutes.

🔹 Question 4: How is AWS Lambda different from EC2 instances?
-> 1) Serverless meaning we dont need to manage servers. It automatically scales and executes code in response to events.
In contrast EC2 instanece requirs manual scaling and updates.

🔹 Question 5: What languages does AWS Lambda support?
-> .net, go , java, python, ruby. Apart from this you can also set your custom env to run your code/function

🔹 Question 6: How can you manage state in an AWS Lambda function?
-> Lambda function is stateless as it does not store data. If we want to store data then we can manage state externally using services like DynamoDB, RDS or storage solutions.

🔹 Question 7: What is the deployment package in AWS Lambda?
-> It is ZIP archive that contains code or any dependencies needed to run the lambda function.. It is uploaded to AWS when function is created. We do not need to upload zip or archieve it, we upload normal files.folders and lambda does/ZIPs it for us.

🔹 Question 8: How does AWS Lambda handle concurrency?
-> It automatically scales horizontally to handle incoming requests.Each function execution is independent and Lambda can run multiple instances of a function concurrency to handle increased loads.


🔹 Question 9: Can AWS Lambda functions access resources inside a VPC?
-> Yes AWS lambda can be configured to access resources inside a VPC. This allows functions to connect securely to private resources like databases.

🔹 Question 10: What is the cold start issue in AWS Lambda? How can it be mitigated?
-> It refers to latency when a fubction is invoked for 1sr time or after being idle. To mitigate this you can use provisioned concurrency, warm up techniques or conside using AS lambda provisioned concurreny feature.

