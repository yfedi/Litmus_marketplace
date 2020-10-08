FROM lyubomyrd/robotics_ros2_docker:latest

WORKDIR /root

ENV Repo_User $REPO_USER
ENV Repo_Pass $REPO_PASS


RUN apt-get update && apt-get install -y \
    openssh-server \
    gdb \
    gdbserver \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get upgrade -y

RUN pip3 install paho-mqtt nats-python

RUN mkdir /var/run/sshd
RUN echo 'root:root' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc

RUN mkdir -p ~/ros_ws/src && \
    cd ~/ros_ws/src && \
    git clone -b docker --single-branch https://$Repo_USER:$Repo_Pass@bitbucket.org/pfeifer-langen-ws1/pl_rospoc.git && \
    cd ~/ros_ws &&\
    colcon build

RUN cd ~/ros_ws &&\
    colcon build

RUN echo "source ~/ros_ws/install/setup.bash" >> ~/.bashrc

RUN echo "PS1='\[\e[33m\][P&L_Controller]\[\e[m\] ${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '" >> ~/.bashrc

CMD [ "/bin/bash" ] 