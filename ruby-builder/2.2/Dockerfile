FROM ramseyinhouse/rubybin AS binscripts

FROM ruby:2.2-alpine

RUN apk add --update --no-cache --virtual .build-dependencies \
    build-base \
    git

WORKDIR /code
VOLUME /code

COPY --from=binscripts /bin/setup /usr/local/bin
COPY --from=binscripts /bin/validate /usr/local/bin
COPY --from=binscripts /bin/build /usr/local/bin

ENV PATH /code/bin:$PATH

CMD validate
