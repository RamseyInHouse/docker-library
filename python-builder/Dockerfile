ARG PYTHON_VERSION

FROM python:${PYTHON_VERSION}-alpine

WORKDIR /code

COPY ./bin /usr/local/bin

ENV PATH /code/bin:$PATH

CMD validate
