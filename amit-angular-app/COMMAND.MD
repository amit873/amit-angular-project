# Build Docker Images
docker build -t amit-angular-app:v1 .

# Run Docker Container
docker run -d -p 4200:4200 --name=amit-angular-app amit-angular-app:v1
docker run -d -p 80:4200 --name=amit-angular-app amit-angular-app:v1
docker run -d -p 8080:4200 --name=amit-angular-app amit-angular-app:v1

# Check Current ulimit
ulimit -n

# Run the Docker Container with --ulimit 
docker run --ulimit nofile=65536:65536 -d your-angular-app


# Check Running Containers
docker ps

# Check All Containers - both Running and Stop
docker ps -a

# Container Logs
docker logs 18acbf0e3804

# Go inside Container
docker exec -it 18acbf0e3804 bash

# Run Container Command from Outside
docker exec -it  18acbf0e3804 ls -ltr /usr/share/nginx/html

# Delete All Containers using Force
docker rm -f $(docker ps -a -q)

# Delete All Images
docker rmi $(docker images -q)

# Tag the image with Docker Hub username and repository name
docker tag amit-angular-app:v1 ameintu/amit-angular-app:v1

# Log in to Docker Hub account 
docker login -u ameintu

# Push the tagged image to Docker Hub
docker push ameintu/amit-angular-app:v1

# Pull Image from Docker Hub
docker pull ameintu/amit-angular-app:v1

# Pull nginx hello Image from Docker Hub
docker pull nginxdemos/hello:latest

# Run Docker Container 
docker run -d -p 4200:4200 --name=amit-angular-app ameintu/amit-angular-app:v1

##### Export the Image as Root User and Import as ec2-user
# As root user
podman save -o angular-app.tar angular-app:latest
chown ec2-user:ec2-user angular-app.tar
# Switch to ec2-user
su - ec2-user
podman load -i angular-app.tar

# Clear npm Cache if needed
npm cache clean --force

##### Push Docker Image to AWS ECR
# Build Docker Image
docker build -t amit-angular-app .

# Retrieve an Authentication Token and Authenticate Docker client to AWS ECR Registry
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 815069173207.dkr.ecr.us-east-1.amazonaws.com

# Tag docker image to push the image to AWS ECR Repository
docker tag amit-angular-app:latest 815069173207.dkr.ecr.us-east-1.amazonaws.com/amit-angular-app:latest

# Push the image to AWS ECR Repository
docker push 815069173207.dkr.ecr.us-east-1.amazonaws.com/amit-angular-app:latest