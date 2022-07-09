### Deploying a Docker Container to ECS

+ `git clone https://github.com/prakhar1989/docker-curriculum.git`
+ `cd docker-curriculum/flask-app`
+ `docker build -t myapp .`
+ `docker run --publish 5000:5000 myapp`
+ From there fill in the name of the repository as myapp and leave everything else default.
+ If you prefer you can also do the above step from the command line like so:
  + ```
    aws ecr create-repository \
    --repository-name myapp \    
    --region us-east-1
    ```
+ `docker tag myapp [use your uri here]`
+ `docker tag myapp 828253152264.dkr.ecr.us-east-1.amazonaws.com/myapp`
+ `aws configure`
+ `aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 170058173433.dkr.ecr.ap-southeast-1.amazonaws.com`
+ `docker push 828253152264.dkr.ecr.us-east-1.amazonaws.com/myapp`
