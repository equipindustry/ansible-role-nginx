FROM equipindustry/python:3.8.0-slim

ARG stage

LABEL NAME ansible-role-nginx-app-$stage

ENV USERNAME developer

ENV PACKAGES bash \
  git \
  gcc \
  libc-dev \
  make \
  musl-dev \
  openssl \
  gcc \
  libgdal20 \
  libev-dev


COPY Pipfile* /usr/src/

# set working directory to /usr/src/
WORKDIR /usr/src/

# install packages
RUN apt-get update -y \
  && apt-get install -y ${PACKAGES} --no-install-recommends \


  # Permissions
  && groupadd -g 500 -r python \
  && groupadd ${USERNAME} \
  && useradd -m -g ${USERNAME} -G python ${USERNAME} \
  && chown -R ${USERNAME}:python /usr/local/bin \
  && su ${USERNAME} \
  && mkdir -p /usr/src \
  && chown -R ${USERNAME} /usr/src \


# pipenv
  && pip3 install pipenv \
  && pipenv install --dev --skip-lock \
  && pipenv shell --fancy \

# clean
  && apt-get clean \
  && apt-get autoremove \
  && apt-get purge \
  && rm -rf /var/lib/apt/lists/*
