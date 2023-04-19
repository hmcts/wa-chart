# chart-ccd

Product chart for Work Allocation.

The Work Allocation services require the CCD services and Azure Service Bus to work. This chart is intended to be used in conjunction with the [ccd chart](https://www.github.com/hmcts/chart-ccd) and teams are expected to have completed the necessary steps to set up an Azure Service Bus in Preview.

## Usage

Add the dependency in Chart.yml

```yaml
dependencies:
  - name: wa-chart
    version: 0.1.4 # See latest release in releases tab
    repository: https://hmctspublic.azurecr.io/helm/v1/repo/
```

Configure the services in values.preview.yaml

```yaml
servicebus:
  enabled: true
  releaseNameOverride: ${SERVICE_NAME}-asb
  resourceGroup: [YOUR RESOURCE GROUP]
  sbNamespace: [YOUR NAMESPACE]
  setup:
    topics:
      - name: ccd-case-events
        subscriptionNeeded: yes
        requiresSession: true

wa:
  enabled: true
  wa-case-event-handler:
    java:
      secrets:
        AZURE_SERVICE_BUS_CONNECTION_STRING:
          secretRef: [YOUR SERVICE BUS SECRET]
          key: connectionString
      environment:
        CCD_URL: http://{{ .Release.Name }}-ccd-data-store-api
        CCD_SEARCH_URL: http://{{ .Release.Name }}-ccd-data-store-api
        ROLE_ASSIGNMENT_URL: http://{{ .Release.Name }}-am-role-assignment-service
        AZURE_SERVICE_BUS_TOPIC_NAME: ${SERVICE_NAME}-asb-ccd-case-events
        AZURE_SERVICE_BUS_SUBSCRIPTION_NAME: ${SERVICE_NAME}-asb-ccd-case-events
        AZURE_SERVICE_BUS_CCD_CASE_EVENTS_SUBSCRIPTION_NAME: ${SERVICE_NAME}-asb-ccd-case-events

ccd-message-publisher:
  java:
    ingressHost: ccd-message-publisher-${SERVICE_FQDN}
    releaseNameOverride: ${SERVICE_NAME}-ccd-message-publisher
    keyVaults:
      ccd:
        secrets:
          - name: AppInsightsInstrumentationKey
            alias: azure.application-insights.instrumentation-key
    environment:
      DATA_STORE_DB_HOST: "{{ .Release.Name }}-postgresql"
      DATA_STORE_DB_NAME: data-store
      DATA_STORE_DB_USERNAME: hmcts
      DATA_STORE_DB_PASSWORD: hmcts
      DATA_STORE_DB_OPTIONS: "?stringtype=unspecified"
      CCD_CASE_EVENTS_DESTINATION: ${SERVICE_NAME}-asb-ccd-case-events
    secrets:
      SERVICE_BUS_CONNECTION_STRING:
        secretRef: [YOUR SERVICE BUS SECRET
        key: connectionString
```

If you are providing your own database then you can disable the one in this chart with: 

```
wa:
  wa:
    postgresql:
      enabled: false
```
