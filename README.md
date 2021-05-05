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

There are 2 options here:

1. [Override](https://helm.sh/docs/chart_template_guide/subcharts_and_globals/#overriding-values-from-a-parent-chart) subchart values.yaml krakendJson key from parent chart values.yaml and paste KrakenD configuration there. 
2. Override subchart usedAsSubchart key to true, create your krakend.json configuration file and put it into parent chart root directory. Create template for configmap in parent chart with the following content:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-kraken-configmap
data:
  krakend.json: |-
{{- if eq .Values.<your_custom_key> "" }}
{{ .Files.Get "krakend.json" | indent 4 }}
{{- else }}
{{ .Values.<your_custom_key> | indent 4 }}
{{- end}}
```
KrakenD configuration can still be modified from values.yaml using <your_custom_key>. This process will substitute subchart configmap with KrakenD configuration with one from parent and mount it to subchart deployment. Now you can keep your KrakenD configuration in your parent chart json file.

   

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

