### How to Run Docker Image from ECR
+ ECR login
    + `aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com`
+  Pull image
    +  `docker pull <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/create-client:latest`
+  Run image
    +  `docker run -it -e AWS_DEFAULT_REGION=ap-southeast-1 -e AWS_ACCESS_KEY_ID=xxxe AWS_SECRET_ACCESS_KEY=xxxx CLIENT_NAME=sandbox <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/create-client:latest`
