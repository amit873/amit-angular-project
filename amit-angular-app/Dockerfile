# Stage 1: Build the Angular application
FROM node:20 AS build

# Check Login User Name
RUN whoami

# Set Working Directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json if available
COPY package.json package-lock.json ./

# Clean up npm cache to reduce the number of open files
RUN npm cache clean --force

# Install dependencies
RUN npm install

# Copy the entire project
COPY . .

# Clean Cache
RUN npm run clean

# Build the Angular app in production mode
RUN npm run build --prod

# Stage 2: Serve the Angular application with Nginx
FROM nginx:alpine

# Set nginx User
#USER nginx
RUN whoami

# Copy the build output to replace the default nginx contents
COPY --from=build /usr/src/app/dist/amit-angular-app/browser /usr/share/nginx/html

# Copy nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 4200
EXPOSE 4200

# Command to run nginx
CMD ["nginx", "-g", "daemon off;"]