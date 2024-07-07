# Swith to Root
sudo -i

# Update the system
yum update -y

# Enable the Podman repository and install Podman
subscription-manager repos --enable=rhel-7-server-extras-rpms
yum install -y podman fuse-overlayfs shadow-utils

# Add the tumss user
adduser tumss
passwd tumss

# Add the tumss user to the wheel group
usermod -aG wheel tumss

# Configure subordinate UID/GID mappings
sh -c 'echo "tumss:100000:65536" >> /etc/subuid'
sh -c 'echo "tumss:100000:65536" >> /etc/subgid'

## It should be like this
$ cat /etc/subuid
root:100000:65536
tumss:100000:65536

$ cat /etc/subgid
root:100000:65536
tumss:100000:65536

# Install Required Tools
yum install -y shadow-utils

# Ensure FUSE is Installed
yum install -y fuse-overlayfs

#### run it via tumss user #####
sudo su - tumss

## Create the necessary configuration directories
mkdir -p ~/.config/containers
mkdir -p ~/.local/share/containers/storage
mkdir -p ~/.local/share/containers/storage/libpod

## Copy the default storage configuration file
cp /etc/containers/storage.conf ~/.config/containers/

## Edit the storage configuration file to set the storage driver and path for the non-root user
vi ~/.config/containers/storage.conf
- Modify storage.conf to:
[storage]
driver = "overlay"
runroot = "/home/tumss/.local/share/containers/storage"
graphroot = "/home/tumss/.local/share/containers/storage"
[storage.options]
mount_program = "/usr/bin/fuse-overlayfs"
:wq!

## Configure Podman to use cgroupfs for tumss
vi ~/.config/containers/containers.conf
[engine]
cgroup_manager = "cgroupfs"
:wq!

# Enable user namespaces (if needed)
- Check namespace
sudo sysctl user.max_user_namespaces

- Set if it is zero
sudo sysctl -w user.max_user_namespaces=15000

# Verify Podman setup for tumss
podman info

# Run a test Podman container as tumss
podman run -d --name mynginx -p 8080:80 nginx
