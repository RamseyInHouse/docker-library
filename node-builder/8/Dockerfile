FROM ramseyinhouse/node-bin AS binscripts

FROM node:8-alpine

WORKDIR /code

COPY --from=binscripts /bin/setup /usr/local/bin
COPY --from=binscripts /bin/validate /usr/local/bin
COPY --from=binscripts /bin/build /usr/local/bin

ENV PATH /code/bin:$PATH

CMD validate
