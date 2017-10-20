FROM ubuntu:16.04

RUN apt-get update
# install curl
RUN apt-get install -y curl

# install node and npm
RUN cd ~ && curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh && \
    bash nodesource_setup.sh && apt-get install -y nodejs
# install build-essential for packages that need to be compiledfrom source
RUN apt-get install -y build-essential
# install postgres and set its config file
RUN apt-get install -y postgresql postgresql-contrib
RUN sed -i 's/peer/trust/' /etc/postgresql/9.5/main/pg_hba.conf && \
    sed -i 's/md5/trust/' /etc/postgresql/9.5/main/pg_hba.conf

RUN apt-get install -y vim