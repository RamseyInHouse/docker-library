ARG NODE_VERSION

FROM node:${NODE_VERSION}-alpine

RUN apk add --update --no-cache --virtual .build-dependencies \
    build-base

WORKDIR /code

COPY ./bin /usr/local/bin

ENV PATH /code/bin:$PATH

CMD validate
