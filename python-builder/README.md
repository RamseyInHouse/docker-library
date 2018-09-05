
# How To Use This Image

## Create a `Dockerfile` in your Python package's project

```dockerfile
FROM ramseyinhouse/python-builder:2.7

COPY . /code

RUN setup
```

Put this file in the root of your project, next to `requirements.txt` or `Pipfile`.

This image includes the following default scripts for your convenience. They live at `/usr/local/bin`. If you want to override them, `/code/bin` is set to the front of the `PATH` to make that easy.
- setup
    - `pip install pipenv`
    - `pipenv install --dev --deploy` - install package dependencies
        - Will use Pipfile if it exists in the package's directory, or will default to requirements.txt.
- validate  - `pipenv run pytest`
- build - Does a `pipenv run python setup.py sdist`, and leaves the `.tar.gz` (source) file in the `/code/dist/` folder inside the container.  Lastly, it echoes the name of the built tar archive.
- publish - automatically publishes your package to the repositories specified in `$UPLOAD_REPOS` ([see this section](#publishing-your-package)).

The default command is the validate script. This means you can build your docker image and run your tests within the container like so:

```console
$ docker build -t your-package .
$ docker run -it --rm your-package
```

## Building your package

You can build your package using the provided command:

```console
$ docker run -it --rm -v "$PWD"/dist:/code/dist your-package build
```

## Publishing your package

If you'd like to have your package automatically published to several repositories, you can do the following:

1. Create a `.pypirc` config file ([see here for more information](https://docs.python.org/2/distutils/packageindex.html#the-pypirc-file)):
```text
[distutils]
index-servers =
    pypi

[pypi]
username:
password:
```

2. Add the `UPLOAD_REPOS`, `ARG`, & `ENV` commands to your package's `Dockerfile`:
```dockerfile
ARG UPLOAD_REPOS
ENV UPLOAD_REPOS=$UPLOAD_REPOS
```

3. Rebuild your package's docker image:
```console
$ docker build --build-arg UPLOAD_REPOS="pypi" -t your-package .
```

4. Run the new image with the `publish` command:
```console
$ docker run -it --rm \
    -v "$PWD"/dist:/code/dist \
    -e "HOME=/home" \
    -v "$HOME/.pypirc:/home/.pypirc" \
    your-package publish
```
**Note**: the key arguments to linking your local `.pypirc` into the container are `-e "HOME=/home" -v "$HOME/.pypirc:/home/.pypirc"`.  This allows the `twine` command within the container to access your locally stored credentials.

### Publishing to a private package repository

1. If you have a private package repository that you would like to publish to (e.g. [Gemfury](https://gemfury.com/)), you can add another config to your `.pypirc`:
```text
[distutils]
index-servers =
    pypi
    fury

[pypi]
username:
password:

[fury]
repository: https://pypi.fury.io/USERNAME/
username: TOKEN
password:
```

2. Then add it to the `UPLOAD_REPOS` env when rebuilding your package's docker image:
```console
$ docker build --build-arg UPLOAD_REPOS="pypi fury" -t your-package .
```


# Image Variants

The builder image comes in two flavors, providing Python versions 2.7 and 3.6:
- `ramseyinhouse/python-builder:2.7`
- `ramseyinhouse/python-builder:3.6`


# License

View [license information](https://www.python.org/download/releases/2.7/license/) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

# How To Build The Images
Example for building a Python 2.7 builder image:
```console
docker build --build-arg PYTHON_VERSION=2.7 -t ramseyinhouse/python-builder:2.7  .
```
