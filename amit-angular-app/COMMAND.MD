# Build Docker Images
docker build -t amit-angular-app:v1 .

# Run Docker Container
docker run -d -p 4200:4200 amit-angular-app:v1 <br/>
docker run -d -p 80:4200 amit-angular-app:v1
docker run -d -p 8080:4200 amit-angular-app:v1

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
docker login

# Push the tagged image to Docker Hub
docker push ameintu/amit-angular-app:v1
