FROM ros:humble

ARG WORKSPACE_DIR=/ros2_ws

ENV DEBIAN_FRONTEND=noninteractive
ENV WORKSPACE_DIR=${WORKSPACE_DIR}

WORKDIR ${WORKSPACE_DIR}
SHELL ["/bin/bash", "-c"]

# Base dependencies
RUN apt update -q \
    && apt upgrade -q -y \
    && apt install -y --no-install-recommends \
        software-properties-common \
        curl \
        build-essential \
        ca-certificates \
        python3-pip \
        xauth \
        gdb \
        htop \
        net-tools \
        iputils-ping \
        tmux \
        xterm \
        nlohmann-json3-dev \
        python3-colcon-common-extensions \
        python3-rosdep \
        python3-colcon-clean \
        python3-vcstool \
        ros-${ROS_DISTRO}-rmw-zenoh-cpp \
        ros-${ROS_DISTRO}-rviz* \
        ros-${ROS_DISTRO}-ros2-control \
        ros-${ROS_DISTRO}-ros2-controllers \
        ros-${ROS_DISTRO}-moveit* \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# Rust (system-wide)
ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
    | sh -s -- -y --no-modify-path --default-toolchain stable \
 && chmod -R a+rw $RUSTUP_HOME $CARGO_HOME

# Ensure workspace exists (ownership handled by VS Code UID sync)
RUN mkdir -p ${WORKSPACE_DIR}

RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /etc/bash.bashrc \
 && echo "export RCUTILS_COLORIZED_OUTPUT=1" >> /etc/bash.bashrc \
 && echo "export RMW_IMPLEMENTATION=rmw_zenoh_cpp" >> /etc/bash.bashrc \
 && echo "alias zenoh='ros2 run rmw_zenoh_cpp rmw_zenohd'" >> /etc/bash.bashrc \
 && echo "alias cb='cd ${WORKSPACE_DIR} && colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release'" >> /etc/bash.bashrc \
 && echo "alias cbi='cd ${WORKSPACE_DIR} && colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-ignore'" >> /etc/bash.bashrc \
 && echo "alias cclean='cd ${WORKSPACE_DIR} && colcon clean workspace -y && ls'" >> /etc/bash.bashrc \
 && echo "alias ssb='source ${WORKSPACE_DIR}/install/setup.bash'" >> /etc/bash.bashrc

CMD ["bash"]
