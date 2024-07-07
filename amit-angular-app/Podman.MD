#### Configure Podman for Non-Root User
# Add ec2-user to the Podman Group
sudo usermod -aG podman ec2-user

# Adjust User Configuration for Podman
sudo mkdir -p /etc/containers
sudo chown ec2-user:ec2-user /etc/containers
sudo mkdir -p /run/user/$(id -u ec2-user)/containers
sudo chown ec2-user:ec2-user /run/user/$(id -u ec2-user)/containers

# Running Podman Commands as ec2-user
su - ec2-user
podman images
podman ps -a

# Share Podman Images Between Root and Non-Root Users
Podman, when run as root, uses /var/lib/containers for its storage. When run as a non-root user, it uses ~/.local/share/containers (or a similar path). To share images between users, consider using the following approach:
### Export and Import Images:
# As root user
podman save -o myimage.tar <image-name>
# Change ownership of the tar file to ec2-user
chown ec2-user:ec2-user myimage.tar
# As ec2-user
podman load -i myimage.tar

