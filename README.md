# chart-ccd

Product chart for Work Allocation.

The Work Allocation services require the CCD services and Azure Service Bus to work. This chart is intended to be used in conjunction with the [ccd chart](https://www.github.com/hmcts/chart-ccd) which provides an Azure Service Bus.

## Usage

Add the dependency in Chart.yml

```yaml
dependencies:
  - name: wa-chart
    version: 0.0.1
    repository: https://hmctspublic.azurecr.io/helm/v1/repo/
```

Configure the services in values.preview.yaml

```yaml

```

