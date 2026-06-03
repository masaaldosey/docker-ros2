# ROS 2 Docker Template

This repository contains a Docker workspace template for [**ROS 2**](https://www.ros.org/). The idea is that a Docker is created for each project at workspace level. Only `src/` folder is mounted into the container so that the workspace can be compiled on the host system as well as inside the container without them interfering.

Here is an overview of the structure of this repository:

```bash
docker-ros2/
├── docker/                           # Docker and Docker-Compose configuration
│   ├── docker-compose.yml              # Base Docker-Compose file containing all the basic Docker set-up
│   ├── docker-compose-gui.yml          # Extends the base Docker-Compose file by X11-forwarding for graphic user interfaces
│   ├── docker-compose-gui-nvidia.yml   # Extends the graphic user interface Docker-Compose file with the Nvidia runtime
│   ├── Dockerfile                      # Dockerfile containing ROS 2 and the base dependencies
│   └── .env                            # Environment variables to be considered by Docker Compose
├── src/                              # Source folder mounted inside the Docker container
├── .devcontainer/                    # Configuration files for containers in Visual Studio Code
└── .vscode/                          # Configuration files for Visual Studio Code
```



## 1. Set-up

After cloning this repository you will have to update the packages inside the workspace. The **configuration** is performed inside the [`docker/.env`](./docker/.env) file:

```bash
WORKSPACE_DIR=/ros2_ws
ROS_DOMAIN_ID=0
YOUR_IP=127.0.0.1
ROBOT_IP=127.0.0.1
ROBOT_HOSTNAME=P500
```

Here you can change the workspace name, network settings as well as user and group IDs.

**Network set-up**: The `ROS_DOMAIN_ID` is used for the ROS 2 Zenoh middleware configuration. The parameter `YOUR_IP` corresponds to the IP that you are using, in case you are running a simulation set it to `127.0.0.1` while for working with a physical robot you will have to set it to the IP assigned to the network interface used for connecting to the robot shown by `$ ifconfig` from the `net-tools` package on your computer. We use it for the Zenoh middleware configuration. The `ROBOT_IP` as well as the `ROBOT_HOSTNAME` are used to configure the `/etc/hosts` file as well as configuring the Zenoh middleware by IP. They should correspond to the IP shown by `$ ifconfig` on the robot as well as to its `$ hostname` and can be set to `127.0.0.1` and an arbitrary hostname in case of the simulation.

In order to be able to run **graphical user interfaces** from inside the Docker you might have to type

```bash
$ xhost +
```

on the **host system**. When using a user with the same name, user and group id as the host system this should not be necessary.



## 2. Running

Either **run the Docker** manually with

```bash
$ cd docker-ros2/
$ docker compose -f docker/docker-compose-gui.yaml up
```

and then connect to the running Docker

```bash
$ cd docker-ros2/
$ docker exec -it ros2_docker bash
```

Alternatively use the corresponding [**Visual Studio Code Dev Containers integration**](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers). In latter case the configuration can be adjusted in `.devcontainer/devcontainer.json`. Using the Docker through Visual Studio Code is much easier and is therefore recommended!


## Credits

this repo is heavily inspired by [docker-for-robotics](https://github.com/2b-t/docker-for-robotics).
