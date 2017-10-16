FROM ubuntu:16.04

RUN apt-get update
# install curl
RUN apt-get install -y curl

# install node and npm
RUN cd ~ && curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh && \
    bash nodesource_setup.sh && apt-get install -y nodejs
# install build-essential for packages that need to be compiledfrom source
RUN apt-get install -y build-essential