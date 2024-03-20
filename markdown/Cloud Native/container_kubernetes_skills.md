# container或kubernetes一些技巧



```
VERSION="1.26.0"
IMAGES=$(ctr -n=k8s.io images list | awk '{if ($1 ~ /^k8s.gcr.io\/(.*):'${VERSION}'.*/) print $1}')

ctr -n=k8s.io images export k8s_${VERSION}.tar.gz $IMAGES
```

```
docker stop `docker ps -a -q`
dokcer rm  `docker ps -a -q`
```

