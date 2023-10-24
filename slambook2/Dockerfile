FROM ubuntu:20.04

#create an user with a /home/user directory
ARG USERNAME=user
ARG PASSWD=test
RUN apt update
RUN apt-get install apt-utils sudo software-properties-common -y

# NODE JS and PYNVIM for use nvim
RUN add-apt-repository ppa:neovim-ppa/unstable && apt update
RUN apt-get install neovim -y
ARG NODE_MAJOR=20
RUN apt-get install -y ca-certificates curl gnupg
RUN mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list

RUN apt-get update
RUN apt-get install nodejs -y
RUN apt-get install python3-pip -y
RUN pip3 install pynvim

RUN apt install tmux -y
RUN apt-get install git -y
RUN apt-get install cmake -y
RUN apt install libopencv-dev -y
RUN apt install libboost-all-dev -y
RUN apt-get install libgoogle-glog-dev libgflags-dev -y
RUN apt-get install libfmt-dev -y

## create an user
RUN useradd --create-home --shell /bin/bash $USERNAME && echo "$USERNAME:$PASSWD" | chpasswd && adduser $USERNAME sudo
RUN echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER $USERNAME
WORKDIR /home/$USERNAME
# this folders are for nvim
RUN mkdir .config
RUN mkdir .local/share -p

COPY 3rdparty /home/user/3rdparty
# Add -y to the last line of install_prerequisites.sh
RUN sudo sed -i '$ s/$/ -y/' /home/user/3rdparty/Pangolin/scripts/install_prerequisites.sh
RUN /home/user/3rdparty/Pangolin/scripts/install_prerequisites.sh recommended
RUN cd /home/user/3rdparty/Pangolin && sudo cmake -B build -GNinja && sudo cmake --build build --target install
RUN cd /home/user/3rdparty/Sophus && sudo cmake -B build -GNinja && sudo cmake --build build --target install
RUN cd /home/user/3rdparty/ceres-solver && sudo cmake -B ceres-bin -GNinja -DCXX11=ON -DCMAKE_INSTALL_PREFIX=/usr/local -DBUILD_EXAMPLES=OFF -DBUILD_TESTING=OFF && sudo cmake --build ceres-bin --target install
RUN cd /home/user/3rdparty/g2o && sudo cmake -B build -GNinja && sudo cmake --build build --target install
