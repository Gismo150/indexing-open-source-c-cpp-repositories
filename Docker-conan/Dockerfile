#Use base-image ubuntu 18.04
FROM ubuntu:18.04
MAINTAINER Daniel Braun<braun.daniel@hotmail.de>

# Update Ubuntu Software repository
RUN apt-get update

# Install general dependencies (software-properties-common required for chmod command below)
RUN apt-get -y install apt-utils git openjdk-11-jre-headless wget software-properties-common

# Install llvm-8 (required for wllvm, need tools llvm-link and llvm-ar)
RUN wget https://apt.llvm.org/llvm.sh && chmod +x llvm.sh && ./llvm.sh 8

# Install Conan dependencies (some conan packages that are build use autotools as it's build system)
RUN apt-get -y install python3-pip autotools-dev autoconf

# Install conan and add additional remotes
RUN yes | pip3 install conan && conan remote add bincrafters https://api.bintray.com/conan/bincrafters/public-conan --insert && conan remote add conan-community https://api.bintray.com/conan/conan-community/conan

# Install compilation dependencies (installing cmake version 3.15.4)
RUN apt-get -y install clang libboost-all-dev libcurl4 libcurl4-openssl-dev && yes | pip3 install wllvm && wget -qO- "https://cmake.org/files/v3.15/cmake-3.15.4-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C /usr/local

# COPY the Container Program, the config file and the conan profile into the container's file system.
COPY programs/config.properties $HOME/Program/
COPY programs/ContainerProgram.jar $HOME/Program/
COPY programs/clang root/.conan/profiles/

#Create a directory that will be specified as a shared folder on runtime.
RUN mkdir shared

# Copy the analysis program into docker
COPY programs/StatisticsAnalysis $HOME/Program/

# Copy dependencies of the analysis program into docker. In particular the shared libraries used by the analysis tool. 
# Intalling phasar itself into docker would require a lot of build time of the docker image, if any changes are made frequently. Thus we simply copy all the pre-build *.so files into the required location.
COPY programs/sharedLibs/llvm /usr/local/lib
COPY programs/sharedLibs/boost_1_66_0 /usr/local/lib
COPY programs/sharedLibs/phasar /usr/local/lib
# Required in order to load the phasar *.so files correctly!
RUN ldconfig

# Docker requires a command for successful build. However, this default command will be overwritten by the collection creation progam.
CMD ["/bin/bash"]

