LXD Setup
=========

# LXC Profile

lxc profile for gui setup

    config:
    environment.DISPLAY: :0
    environment.PULSE_SERVER: unix:/home/ubuntu/pulse-native
    nvidia.driver.capabilities: all
    nvidia.runtime: "true"
    user.user-data: |
        #cloud-config
        runcmd:
        - 'sed -i "s/; enable-shm = yes/enable-shm = no/g" /etc/pulse/client.conf'
        packages:
        - x11-apps
        - mesa-utils
        - pulseaudio
    description: LXC profile for ROS
    devices:
    PASocket1:
        bind: container
        connect: unix:/run/user/1000/pulse/native
        gid: "1000"
        listen: unix:/home/ubuntu/pulse-native
        mode: "0777"
        security.gid: "1000"
        security.uid: "1000"
        type: proxy
        uid: "1000"
    X0:
        bind: container
        connect: unix:@/tmp/.X11-unix/X1
        listen: unix:@/tmp/.X11-unix/X0
        security.gid: "1000"
        security.uid: "1000"
        type: proxy
    eth0:
        name: eth0
        network: lxdbr0
        type: nic
    mygpu:
        type: gpu
    root:
        path: /
        pool: default
        type: disk
    name: ros
    used_by:

# IP Configuration

Setting up static to containers

    devices:
    eth0:
        ipv4.address: 192.168.1.114/24  - add this line and enter any ip address you like
        name: eth0
        nictype: macvlan - this is my setting
        parent: enp0s25
        type: nic
# Proxy

Setting for port forwaring host <-> container for communication between ros / ros-bridge and unity3d 

## Setup

    lxc config device add CONTAINER PROXY_NAME proxy listen=tcp:HOST:PORT connect=tcp:CONTAINER_IP:PORT
- lxc config device add noetic ros11311 proxy listen=tcp:0.0.0.0:11311 connect=a.b.c.d:11311

## Remove

    lxc config device remove CONTAINER PROXY_NAME
- lxc config device remove noetic ros11311

!!! Important 
    https://www.linode.com/docs/guides/beginners-guide-to-lxd-reverse-proxy/

# Mounting Folder

## Adding
    lxc config device add <CONTAINER_NAME> <DEVICE_NAME> disk source=/home/wolf/codebase/ros/turtlebot3-container/turtlebot3_ws/ path=/home/ubuntu/turtlebot3_ws

## Removing

    lxc config device remove <CONTAINER_NAME> <DEVICE_NAME>

# Privilidged

    lxc config set <CONTAINER_NAME> security.privileged true
    lxc config set <CONTAINER_NAME> security.privileged false

# Show Config

    lxc config show <CONTAINER_NAME>


# SSH

If our public key is stored at ~/.ssh/id_rsa.pub, we'll simply create an authorized_keys file with our public key in it, and push it

    ubuntu@lxhost:~$ cp ~/.ssh/id_rsa.pub /tmp/authorized_keys
    ubuntu@lxhost:~$ lxc file push /tmp/authorized_keys <CONTAINER_NAME>/home/ubuntu/.ssh/authorized_keys -p

> create proxy / port forward for port 22

    lxc config device add <CONTAINER_NAME> proxy22 proxy connect=tcp:127.0.0.1:22 listen=tcp:0.0.0.0:2222

!!! Important
    https://www.arnatious.com/blog/lxc/
