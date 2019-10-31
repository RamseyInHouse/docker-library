# How to use this image

This image provides a few helpful web features that you can use in developing web applications on your local machine. It includes...
* A self-signed certificate to allow traffic to be served over HTTPS.
* A proxy from port 443 to any application running on port 3000 on your machines localhost.

The most common use case for this image would be to redirect traffic from the web to hit your local machine. Say for example you have an application that in production runs on the URL `https://yourawesomewebsite.com`. Usually, when running this locally you would spin up your server and type something like `localhost:3000` in your browser to load up the application. This usually works great but if you need to test the interaction between your website and another website, sometimes hard-coded links can make that difficult. This image aims to help solve that problem.

## Setup
### Preparation
Before using this image you need to set up any domains you want to use to be redirected to your local machine. Open up the file `/etc/hosts` in an editor with sudo permissions, and add the following line.

```sh
127.0.0.1 yourawesomewebsite.com yourothercoolwebsite.net
```
where every entry after the IP is a domain you want to be redirected to localhost.

## Running the Image
### One-off run
You can use `docker run -p 443:443 ramseyinhouse/nginx-ssl` to do run the image.

### Docker compose
You can also include this file in a docker-compose setup with the following entry as a service.

```yaml
nginx:
  image: ramseyinhouse/nginx-ssl
  ports:
    - "443:443"
```

# How to build the images.
Example for building a nginx-ssl image:
```console
docker build -t ramseyinhouse/nginx-ssl  .
```
