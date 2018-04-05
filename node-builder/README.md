
# How to use this image

## Create a `Dockerfile` in your Node package's project

```dockerfile
FROM ramseyinhouse/node-builder:9

COPY .yarnrc /code
COPY package.json /code
COPY yarn.lock /code

RUN setup \
    && apk del .build-dependencies

COPY . /code
```

Put this file in the root of your project.

This image includes the following default scripts for your convenience. They live at `/usr/local/bin`. If you want to override them, `/code/bin` is set to the front of the `PATH` to make that easy.
- setup - `yarn install`
- validate  - `yarn test`
- build - Does a `yarn build` followed by `yarn pack`, and leaves the generated artifact in the `/code/dist/` folder inside the container. (The dist folder will not be created by this process. It must exist)

The default command is the validate script. This means you can build your image, and run your tests within the container like so:

```console
$ docker build -t your-package .
$ docker run -it --rm your-package
```

## Building your package

To build your package using the provided script:

```console
$ docker run -it --rm -v "$PWD"/dist:/code/dist your-package build
```

# Image Variants

The builder image comes in three flavors, providing node versions 6, 8 and 9:
- `ramseyinhouse/node-builder:9`
- `ramseyinhouse/node-builder:8`
- `ramseyinhouse/node-builder:6`


# License

View [license information](https://raw.githubusercontent.com/nodejs/node/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

# How to build the images.
Example for building a node 9 image:
```console
docker build --build-arg NODE_VERSION=9 -t ramseyinhouse/node-builder:2.5  .
```
