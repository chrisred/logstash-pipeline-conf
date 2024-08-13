# Logstash Pipelines

Logstash pipline configurations.

## LogicMonitor ServiceNow

The `logcimonitor-servicenow.conf` pipeline accepts events from [LogicMonitor](https://www.logicmonitor.com/) and sends them to the [ServiceNow](https://docs.servicenow.com/csh?topicname=c_EM.html&version=latest) Event Management [web service API](https://docs.servicenow.com/csh?topicname=send-events-via-web-service.html&version=latest).

### Usage

Edit `logcimonitor-servicenow.conf`, the fields that require editing are at the start of the file. The `json_encode` [filter plugin](https://www.elastic.co/guide/en/logstash/current/plugins-filters-json_encode.html) must be available, this can be installed with `bin/logstash-plugin install logstash-filter-json_encode`.

Create a [Custom HTTP Delivery](https://www.logicmonitor.com/support/alerts/integrations/custom-http-delivery) integration in LogicMonitor. "Alert Data" should be in the JSON format below. An optional `snow_event_class` JSON property maps to the `Source instance` event field in ServiceNow. For example add `"snow_event_class": "sandbox"` to identify events from a LogicMonitor sandbox test environment.

```json
{
    "host": "##HOST##",
    "hostname": "##HOSTNAME##",
    "website": "##WEBSITE##",
    "agentid": "##AGENTID##",
    "internalid": "##INTERNALID##",
    "alertid" : "##ALERTID##",
    "alerttype": "##ALERTTYPE##",
    "alertstatus": "##ALERTSTATUS##",
    "level": "##LEVEL##",
    "datasource": "##DATASOURCE##",
    "dsname": "##DSNAME##",
    "instance": "##INSTANCE##",
    "datapoint": "##DATAPOINT##",
    "value": "##VALUE##",
    "date" : "##DATE##",
    "start": "##START##",
    "duration": "##DURATION##",
    "end": "##END##",
    "threshold": "##THRESHOLD##",
    "message" : "##MESSAGE##",
    "hostgroup": "##HOSTGROUP##",
    "wineventid": "##EVENTCODE##",
    "webcheckurl": "##URL##"
}
```

> [!NOTE]
> To avoid issues with parsing certain time zones the `GMT (No daylight saving)` or `UTC` zone should be set under [Account Information](https://www.logicmonitor.com/support/settings/account-information/portal-settings) in LogicMonitor. LogicMonitor uses an [abbreviated time zone](https://en.wikipedia.org/wiki/List_of_time_zone_abbreviations) string for the [tokens](https://www.logicmonitor.com/support/logicmodules/about-logicmodules/tokens-available-in-datasource-alert-messages) used in the alert data. Common abbreviated zones such as `BST` and `CST` are not unique.
>
> If the date cannot be parsed from the LogicMonitor event, the time it was received by the pipeline will be used.