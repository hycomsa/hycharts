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

### Using KrakenD 

#### - Standalone chart

Just modify krakend.json. 

#### - As a dependency

[Override](https://helm.sh/docs/chart_template_guide/subcharts_and_globals/#overriding-values-from-a-parent-chart) subchart values.yaml krakendJson key from parent chart values.yaml and paste KrakenD configuration there. 

### Using KrakenD Flexible Configuration

#### - Standalone chart

Enable Flexible Config in values.yaml and put your components into flexible-configuration directory with settings, partials and templates respectively as described by [official documentation](https://www.krakend.io/docs/configuration/flexible-config/).

#### - As a dependency 

1. [Override](https://helm.sh/docs/chart_template_guide/subcharts_and_globals/#overriding-values-from-a-parent-chart) subchart values.yaml from parent chart to enable flexible configuration and override krakendJson to update gateway configuration. 
   Parent values.yaml should have following keys:
   
```yaml
krakend:
  flexible_config:
    enabled: true
  krakendJson: "<paste you configuration here>"
```
2. Create directory for flexible configuration and subdirectories for partials, settings, templates.
   Parent chart directory structure should look like this:

```text
 <your_chart>
    └── flexible-configuration
         ├── settings
         ├── partials
         └── templates
```
3. Copy templates for [settings-configmap.yaml](https://raw.githubusercontent.com/hycomsa/hycharts/master/krakend-helm/templates/settings-configmap.yaml), [partials-configmap.yaml](https://raw.githubusercontent.com/hycomsa/hycharts/master/krakend-helm/templates/partials-configmap.yaml) and [templates-configmap.yaml](https://raw.githubusercontent.com/hycomsa/hycharts/master/krakend-helm/templates/templates-configmap.yaml) and put them into parent chart templates directory. Modify the first line in each file to:
```text
{{- if .Values.krakend.flexible_config.enabled }}
```

