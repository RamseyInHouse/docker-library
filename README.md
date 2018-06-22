# Ramsey In-House docker-library

This is the home for `Dockerfile`s behind the images available at https://hub.docker.com/u/ramseyinhouse

- They are created specifically for building/developing libraries such as ruby gems and node/php packages. They are not intended for running applications.
- They are intended to provide a common interface for a CI environment. Regardless of the language, a build server can `docker run -v "$PWD:/code/dist" image-name build` and deploy the returned artifact out of the `dist/` folder.
- They are intended to produce small images. Hence the alpine bases.

Further documentation is provided per language:
 - [ruby](ruby-builder/README.md)
 - [node](node-builder/README.md)
