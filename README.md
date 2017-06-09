# syslog-nozzle-binary

[firehose-to-syslog nozzle](https://github.com/cloudfoundry-community/firehose-to-syslog) deployment using binary buildpack


## Deployment

1. Clone this repo
2. Download [Linux x64 Binary](https://github.com/cloudfoundry-community/firehose-to-syslog/releases/tag/3.0.0), rename execution file to `firehose-to-syslog`and put into _firehose-to-syslog_ folder, set correct execute permission. 
3. Update `manifest.yml` with proper values. 
4. Run `cf push`.

## UAA Client

Follow [Use Client id / Client Secret](https://github.com/cloudfoundry-community/firehose-to-syslog#push-as-an-app-to-cloud-foundry) to creat UAA client

```
uaac target https://uaa.[cf system domain] --skip-ssl-validation
uaac token client get admin -s [admin-client-secret]
uaac client add firehose-to-syslog \
  --name firehose-to-syslog \
  --secret [your_client_secret] \
  --authorized_grant_types client_credentials,refresh_token \
  --authorities doppler.firehose,cloud_controller.admin
```

## Troubleshoot

Sicne latest syslog nozzle introduce new function that nozzle emit is own stat to syslog server, instead of printing as stdout, search syslog side to get its stats to confirm nozzle producing the events

for example filter by `firehose_to_syslog_stats`:

```
doppler: {"LogMessage":149,"by_sec_Events":0,"cf_origin":"firehose","event_type":"firehose_to_syslog_stats","level":"info","msg":"Statistic for firehose to syslog","time":"2017-06-07T16:44:04Z","total_count":149}
```


## Reference


