

## Push images to Docker registry

```shell
## find the image to push
$ docker images

## tag image
$ docker tag ce160677a129 username/nodejs-webapp-demo

## push image
$ docker push username/nodejs-webapp-demo
```

## Configure Docker credential store

```shell
$ cat $HOME/.docker/config.json

```
## Troubleshooting

### 

```shell
$ Docker login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: username
Password: 
Error saving credentials: error storing credentials - err: exec: "docker-credential-desktop": executable file not found in $PATH, out: ``
```

**Solution:**

Delete `"credsStore": "desktop"` from `$HOME/.docker/config.json` and save the file.