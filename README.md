## Pre-requisite

* Linux dest: Ubuntu:22.04
* Install: `sudo apt install openjdk-17-jre-headless scala -y`
* Version:
    ```
    $ java -version
    openjdk version "17.0.8.1" 2023-08-24
    OpenJDK Runtime Environment (build 17.0.8.1+1-Ubuntu-0ubuntu120.04)
    OpenJDK 64-Bit Server VM (build 17.0.8.1+1-Ubuntu-0ubuntu120.04, mixed mode, sharing)
    ```
    ```
    $ sbt sbtVersion
    [info] welcome to sbt 1.9.7 (Private Build Java 17.0.8.1)
    ```

## Hello world with Play Framework (scala)

Follow [Play Hello World Web Tutorial for Scala](https://github.com/playframework/play-samples/tree/3.0.x/play-scala-hello-world-tutorial#play-hello-world-web-tutorial-for-scala).

## Run an application in production mode

Follow [Deploying your application](https://www.playframework.com/documentation/3.0.x/Deploying#Deploying-your-application).

### Configuring the application secret

Generate an application secret.

```
sbt playGenerateSecret
```

Create `production.conf`.

```
include "application"
play.http.secret.key="<secret>"
play.filters.hosts {
  allowed = ["."]
}
```

※  More details about the Play filter is available [here](https://www.playframework.com/documentation/3.0.x/AllowedHostsFilter).

### Running a production server in place

```
sbt clean stage
```

Edit `play.service` unit file and copy it to `/etc/systemd/system` directory.

Start `play` service.

```
sudo systemctl enable --now play.service
```
Check `localhost:9000`.

### Setting up a front end HTTP server (Nginx)

Follow [Set up with nginx](https://www.playframework.com/documentation/3.0.x/HTTPServer#Set-up-with-nginx).

The `/etc/nginx/nginx.conf` file should define upstream and server block in http directive like this:

```
upstream playapp {
  server 127.0.0.1:9000;
}

server {
  listen 80;
  server_name localhost;
  location / {
    proxy_pass http://playapp;
  }
}
```

Check `localhost`.
