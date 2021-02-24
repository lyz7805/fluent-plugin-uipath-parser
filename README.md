# Fluent::Plugin::Uipath::Parser

Fluentd Parser plugin to parse UiPath Robot execution log

## Installation

install it yourself as:

    $ fluent-gem install fluent-plugin-uipath-parser

## Configration
* **encoding** (string) (optional): Encoding of log file.
  * Default value: Windows-31J.

## Example Configuration

```
<source>
  @type tail
  path "#{ENV['LOCALAPPDATA']}/UiPath/Logs/%Y-%m-%d_Execution.log"
  pos_file "#{ENV['LOCALAPPDATA']}/UiPath/Logs/Execution_log.pos"
  # Deprecated parameter. Use <parse> instead. https://docs.fluentd.org/input/tail#format
  # format uipath
  # encoding Windows-31J
  tag uipath
  <parse>
    @type uipath
    encoding UTF-8
  </parse>
</source>

<filter uipath>
  @type stdout
</filter>

<match uipath>
  @type elasticsearch
  host localhost
  port 9200
  logstash_format true
  include_timestamp true
  logstash_prefix log-fluentd-${tag}
  logstash_dateformat %Y-%m-%d
</match>

<match uipath>
  @type s3
  aws_key_id <AccessKey ID>
  aws_sec_key <Seclet AccessKey>
  s3_bucket <Bucket>
  s3_region <Region>
  path logs/
  time_slice_format %Y%m%d/%Y%m%d-%H
  time_slice_wait 5m
</match>
```

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
