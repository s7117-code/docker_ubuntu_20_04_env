FROM ubuntu:20.04

# ENV
ENV DEBIAN_FRONTEND=noninteractive
ENV DISPLAY=host.docker.internal:0.0

# Set Timezone
ENV TZ=America/New_York
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Update
RUN apt-get update -y
RUN apt-get upgrade -y

# Install Packages
RUN apt-get install apt-utils \
  sudo vim git wget curl locales ssh htop \
  build-essential libssl-dev zlib1g-dev libbz2-dev \
  libreadline-dev libsqlite3-dev wget curl llvm \
  libncurses5-dev libncursesw5-dev xz-utils tk-dev \
  libffi-dev liblzma-dev \
  g++ make unzip zip \
  dos2unix x11-apps -y

# Install Java and Download Server File
RUN apt-get install openjdk-17-jdk -y

# Set locale
RUN locale-gen en_US.UTF-8

# Create a new user
RUN adduser user --shell /bin/bash --disabled-password --home /home/user --gecos ""
RUN usermod -aG sudo user
RUN passwd --lock root
RUN echo 'user:change-this-2025' | chpasswd

# Get the os configs
RUN su - user -c "git clone https://github.com/s7117/.dotfiles.git /home/user/.dotfiles"

USER user
WORKDIR /home/user

# Get the server and start it up
RUN wget https://www.curseforge.com/api/v1/mods/426926/files/4034008/download
RUN mv /home/user/download /home/user/server.zip
RUN unzip server.zip -d /home/user


#RUN java -Xmx8G -Xms1G -jar server.jar --nogui
RUN echo "eula=true" > eula.txt

# Command
CMD sh startserver.sh
