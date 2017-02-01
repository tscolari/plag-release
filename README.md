# Plag Bosh Release


Get metrics from your log files and push them to datadog


### Assumptions:

* The thing you want to get metrics from writing log files using the lager library/format (https://github.com/cloudfoundry/lager)
* The thing you want to get metrics from logs using `start` and `end` (or similar) for their sessions. e.g:
  ```
    logger := lager.NewLogger("my-app")
    ...
    func theThingIwantMetrics(loger lagger.Logger, ...) {
      logger = logger.Session("the-thing")
      logger.Debug("start")
      defer logger.Debug("end")
    }
  ```
  In this case you're going to get metrics from `my-app.the-thing`. 
  Valid `start` `end` combinations -> https://github.com/tscolari/plag/blob/master/parser/parser.go#L13-L14


### Usage:

Deploy the `plag` job collocated with the things you want metric from, and configure the properties:

```
properties:
  plag:
    datadog-api-key: 1111111111
    datadog-app-key: 2222222222
    metrics:
    - message: my-app.the-thing
      datadog-metric-name: dev-deployment.the-thing
      log-file: /var/vcap/sys/log/my-app/output.log
    - message: my-app.another-thing
      datadog-metric-name: dev-deployment.another-thing
      log-file: /var/vcap/sys/log/my-app/output.log
    - message: my-app.another-thing.special-case
      datadog-metric-name: dev-deployment.another-thing.special-case
      log-file: /var/vcap/sys/log/my-app/output.log
      ...
```


Check https://github.com/tscolari/plag-release/tree/master/manifests for more examples
