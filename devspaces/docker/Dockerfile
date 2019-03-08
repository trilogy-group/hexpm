FROM ubuntu:18.04

LABEL description="This file generates a docker image containing everything needed to build and run Hexpm application"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get install -y wget \
    build-essential \
    postgresql \
    postgresql-contrib \
    curl \
    git && \
    wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb && \
    dpkg -i erlang-solutions_1.0_all.deb && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && \
    apt-get -y install esl-erlang elixir nodejs git python yarn && \
    update-rc.d postgresql enable && \
    apt-get clean   

ENV LANG=C.UTF-8

USER postgres
RUN service postgresql start && \
    psql -c "ALTER USER postgres PASSWORD 'postgres';" && \
    psql -c "CREATE DATABASE hexpm_dev;" && \
    psql -c "GRANT ALL PRIVILEGES ON DATABASE hexpm_dev TO postgres;" && \
    psql -c "ALTER USER postgres WITH SUPERUSER;" && \
    psql -c "CREATE DATABASE hexpm_test" && \
    psql -c "GRANT ALL PRIVILEGES ON DATABASE hexpm_test TO postgres;"

USER root

WORKDIR /data

CMD service postgresql start && tail -f /dev/null
