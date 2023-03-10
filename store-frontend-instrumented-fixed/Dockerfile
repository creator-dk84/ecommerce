# syntax = docker/dockerfile:1.2
# ^ This enables the new BuildKit stable syntax which can be
# run with the DOCKER_BUILDKIT=1 environment variable in your
# docker build command (see build.sh)
FROM ruby:2.7.2-slim-buster

# Update, upgrade, and cleanup debian packages
RUN apt-get update && \
    apt-get upgrade --yes && \
    apt-get install -y --no-install-recommends shared-mime-info curl build-essential git-core libsqlite3-dev libpq-dev && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && \
    apt-get install -y nodejs yarn && \
    rm -rf /var/lib/apt/lists/*

COPY . /spree
WORKDIR /spree


# COPY ./config/database.yml /spree/sandbox/config/database.yml
#COPY ./store-frontend /spree/store-frontend
RUN chgrp -R 0 /spree && \
    chmod -R g=u /spree

# Copy in our frontend and run bundle
RUN cd store-frontend && \
      bundle update nokogiri rake marcel mimemagic && \
      bundle install --full-index && \
      yarn install

CMD ["sh", "docker-entrypoint.sh"]
