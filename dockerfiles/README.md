# Docker Images

## Build docker image locally
```
docker image build dockerfiles/ -t corgiponcho/iso:{TAG}
```
## Push image to docker hub
```
docker login
docker push corgiponcho/iso:{TAG}
```

## Run docker image locally
```
docker run -i -t corgiponcho/iso:1.0.1 /bin/bash
```