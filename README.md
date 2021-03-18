# HyCloud Charts Repo

This is an HyCloud charts repository.

### How It Works
create and publish docs:

```console
$ helm create mychart
$ helm package mychart
$ mv mychart-0.1.0.tgz docs
$ helm repo index docs --url https://hy-cloud-marcin-masternak.github.io/hycharts
$ git add -i
$ git commit -av
$ git push origin master
```

From there, I can do a 
`helm repo add hycharts https://hy-cloud-marcin-masternak.github.io/hycharts`