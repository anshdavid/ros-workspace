LXD Setup
=========

LXD, which is written in Go, creates a system daemon that apps can access locally using a Unix socket, or over the network via HTTPS. LXD's main selling points include the following: A host can run many LXC containers using only a single system daemon, which simplifies management and reduces overhead

> Reference documentaion for the following instruction:
    1. [Ros LXD][__ROS_LXD__]
    2. [Gui apps inside LXD][__GUI_LXD__]
    3. [Ros development LXD][__ROS_DEV_LXD__]

# LXC Profile

LXD profiles are container configuration files. Below a profile to run ros and gui apps inside container

    config:
    environment.DISPLAY: :0
    environment.PULSE_SERVER: unix:/home/ubuntu/pulse-native
    nvidia.driver.capabilities: all
    nvidia.runtime: "true"
    raw.idmap: both __UID__ 1000
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
            connect: unix:@/tmp/.X11-unix/__X1__
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

### NOTE:

- Note that field `__X1__` should be adapted for your case. The number is derived from the environment variable `$DISPLAY` on the host. If the value is `:1`, use `X1`. If the value is `:0`, change the profile to `X0` instead.
- `__UID__` user id on host machine to be mapped to user id inside the container. This helps in read/write permissions when sharing files between container and the host
# IP Configuration

Setting up static ip for containers

    devices:
        eth0:
            ipv4.address: 192.168.1.114/24  - add this line and enter any ip address you like
            name: eth0
            nictype: ...
            parent: ...
            type: nic
# Devices

## Port forwarding / Proxy Device
To setup port forwaring between container host and container, proxy devices need to be added

> **lxc config device add CONTAINER_NAME PROXY_DEVICE_NAME proxy listen=tcp:HOST:PORT connect=tcp:CONTAINER_IP:PORT**
- Example: lxc config device add container-noetic ros11311 proxy listen=tcp:0.0.0.0:11311 connect=a.b.c.d:11311

## Sharing workspace / Mounting Folder

> **lxc config device add CONTAINER_NAME DISK_DEVICE_NAME disk source=<PATH/TO/FOLDER/ON/HOST> path=PATH/TO/FOLDER/ON/CONTAINER**
- Example: lxc config device add container-noetic catkin_ws disk source=/ros/ros-workspace/robot_ws/ path=/home/ubuntu/robot_ws
## Remove

To remove a device attached to a container:

> **lxc config device remove CONTAINER_NAME DEVICE_NAME**

# Privilidged Container

Setting container to **priviledged mode** should be avoided

> lxc config set CONTAINER_NAME security.privileged true

> lxc config set CONTAINER_NAME security.privileged false

# SSH

Depending on how we like to develop, we may prefer to ssh in (for instance, to use [vscode’s remote tools][__VSCODE_REMOTE__]).

If our public key is stored at ~/.ssh/id_rsa.pub, we'll simply create an authorized_keys file with our public key in it, and push it

    ubuntu@lxhost:~$ cp ~/.ssh/id_rsa.pub /tmp/authorized_keys
    ubuntu@lxhost:~$ lxc file push /tmp/authorized_keys <CONTAINER_NAME>/home/ubuntu/.ssh/authorized_keys -p

<!-- create proxy / port forward for port 22
lxc config device add <CONTAINER_NAME> proxy22 proxy connect=tcp:127.0.0.1:22 listen=tcp:0.0.0.0:2222 -->

---

[__ROS_LXD__]: https://ubuntu.com/blog/installing-ros-in-lxd
[__ROS_DEV_LXD__]: https://ubuntu.com/blog/ros-development-with-lxd
[__GUI_LXD__]: https://blog.simos.info/running-x11-software-in-lxd-containers/
[__LINODE_LXC_REV_PROXY__]: https://www.linode.com/docs/guides/beginners-guide-to-lxd-reverse-proxy/
[__SSH_1__]: https://askubuntu.com/questions/1106369/how-to-ssh-into-a-lxd-guest
[__VSCODE_REMOTE__]: https://code.visualstudio.com/docs/remote/ssh
[__LXD_1__]: https://www.arnatious.com/blog/lxc/
