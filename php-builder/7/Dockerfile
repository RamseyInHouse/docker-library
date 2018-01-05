FROM ramseyinhouse/php-bin AS binscripts

FROM php:7-cli-alpine3.4

RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /code

COPY --from=binscripts /bin/setup /usr/local/bin
COPY --from=binscripts /bin/validate /usr/local/bin
COPY --from=binscripts /bin/build /usr/local/bin

ENV PATH /code/bin:$PATH

CMD validate
