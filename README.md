# HyCloud Charts Repo

This is an HyCloud charts repository.

### How It Works
create and publish docs:

```console
$ helm create mychart
$ helm package mychart
$ mv mychart-0.1.0.tgz docs
$ helm repo index docs --url https://hycomsa.github.io/hycharts/
$ git add -i
$ git commit -av
$ git push origin master
```

From there, I can do a 
`helm repo add hycharts https://hycomsa.github.io/hycharts/`

### Using KrakenD Flexible Configuration

Enable Flexible Config in values yaml and put your components into flexible-configuration directory with settings, partials and templates respectively as described by [official documentation](https://www.krakend.io/docs/configuration/flexible-config/).

