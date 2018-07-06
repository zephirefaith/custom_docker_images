# inheriting from fetch + indigo + viz + GPU support w/nvidia-docker2
FROM docker.cogrob.com/dockerlogin_fetch_nvidia

# BAXTER SDK
#  catkin workspace
ENV CATKIN_WS=/root/ws_fetch
RUN mkdir -p ${CATKIN_WS}/src
WORKDIR ${CATKIN_WS}/src

# install deps
RUN apt-get update && apt-get install -q -y \
    git-core \
    python-argparse \
    python-wstool \
    python-vcstools \
    python-rosdep \
    ros-indigo-control-msgs \
    ros-indigo-joystick-drivers

# clone source code
RUN wstool init . \
    && wstool merge \
    https://raw.githubusercontent.com/RethinkRobotics/baxter/master/baxter_sdk.rosinstall \
    && wstool update

# install build dependacies
RUN apt-get -qq update && \
    apt-get -qq install -y \
    python-catkin-tools && \
    rosdep update && \
    rosdep install \
    -ry \
    --from-paths . \
    --ignore-src \
    --rosdistro ${ROS_DISTRO} \
    --as-root=apt:false && \
    rm -rf /var/lib/apt/lists/*

# build packages from source
WORKDIR $CATKIN_WS
ENV TERM xterm
ENV PYTHONIOENCODING UTF-8

RUN catkin config \
    --extend /opt/ros/$ROS_DISTRO \
    --install \
    --cmake-args \
    -DCMAKE_BUILD_TYPE=Release && \
    catkin build

