FROM selenium/standalone-chrome

LABEL Fuchi Corp <fuchicorpsolution@gmail.com>

## Version of the Docker
ENV DOCKERVERSION="18.03.1-ce"
ENV MAVEN_VERSION="3.6.3"
ENV M2_HOME="/opt/apache-maven-3.6.3"

USER root
## Downloading mvn code
RUN cd /opt && wget http://apache.spinellicreations.com/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && tar -xvzf apache-maven-${MAVEN_VERSION}-bin.tar.gz \ 
  && sudo update-alternatives --install "/usr/bin/mvn" "mvn" "${M2_HOME}/bin/mvn" 0 \ 
  && sudo update-alternatives --set mvn ${M2_HOME}/bin/mvn \ 
  && sudo mkdir /etc/bash_completion.d -p && rm -rf apache-maven-${MAVEN_VERSION}-bin.tar.gz \ 
  && sudo wget https://raw.github.com/dimaj/maven-bash-completion/master/bash_completion.bash --output-document /etc/bash_completion.d/mvn


## Installing open JDK 
RUN sudo apt update && sudo apt install -y openjdk-8-jdk 

## Download docker cli
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKERVERSION}.tgz \
  && tar xzvf docker-${DOCKERVERSION}.tgz --strip 1 -C /usr/local/bin docker/docker \
  && rm docker-${DOCKERVERSION}.tgz

## Install kubectl
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubectl \
    && chmod +x ./kubectl \
    && mv ./kubectl /usr/local/bin/kubectl \
    && mkdir ~/.kube

## Coping the script to set kube config
COPY . /scripts/Dockerfile/

## Running the script to set ~/.kube/config file
CMD su - root -c "sh /scripts/Dockerfile/set-config.sh"
