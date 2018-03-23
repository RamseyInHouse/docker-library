
# How to use this image

## Create a `Dockerfile` in your Ruby gem's project

```dockerfile
FROM ramseyinhouse/ruby-builder-2.2

COPY your-gem.gemspec /code/your-gem.gemspec
COPY Gemfile /code/Gemfile
COPY Gemfile.lock /code/Gemfile.lock
COPY lib/your-gem/version.rb /code/lib/your-gem/version.rb

RUN setup

COPY . /code

```

Put this file in the root of your app, next to the `Gemfile`.

This image includes the following default scripts for your convenience. They live at `/usr/local/bin`. If you want to override them, `/code/bin` is set to the front of the `PATH` to make that easy.
- setup - `bundle install`
- validate  - `bundle exec rake`
- build - Does a `gem build`, and leaves the `.gem` file in the `/code/dist/` folder inside the container. (The dist folder will not be created by this process. It must exist)

The default command is the validate script. This means you can build your image, and run your tests within the container like so:

```console
$ docker build -t your-gem .
$ docker run -it --rm your-gem
```

## Building your gem

To build your gem using the provided script:

```console
$ docker run -it --rm -v "$PWD":/code/dist your-gem build
```

# Image Variants

The builder image comes in three flavors, providing ruby versions 2.2, 2.3 and 2.4:
- `ramseyinhouse/ruby-builder-2.2`
- `ramseyinhouse/ruby-builder-2.3`
- `ramseyinhouse/ruby-builder-2.4`


# License

View [license information](https://www.ruby-lang.org/en/about/license.txt) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

# Credit
Much of this was gratefully adopted from the official ruby image [documentation](https://github.com/docker-library/docs/blob/master/ruby/README.md).

# How to build the images.
Example for building a ruby 2.5 image:
```console
docker build --build-arg RUBY_VERSION=2.5 -t ramseyinhouse/ruby-builder-2.5  .
```
