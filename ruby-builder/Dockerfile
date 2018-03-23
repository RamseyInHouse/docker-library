ARG RUBY_VERSION

FROM ruby:${RUBY_VERSION}-alpine

RUN apk add --update --no-cache --virtual .build-dependencies \
    build-base \
    git

WORKDIR /code

COPY ./bin /usr/local/bin

ENV PATH /code/bin:$PATH

CMD validate
