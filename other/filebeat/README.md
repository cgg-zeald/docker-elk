
# Running

This project was configured to ingest only apache logs using logstash with grok, it has an additional configuration to get the `clientip` and convert it to GeoIP.

In order to get the mapping without issue here are the following steps needed to be done.

 1. Download the file beat
 1. Download the configuration `<proj>/other/filebeat/filebeat.yml`
 1. Download the log files (for .gz access log just name it to `access_XXX.log`)
 1. Adjust the filebeat.yml to the logs


```
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.14.1-linux-x86_64.tar.gz
tar xzvf filebeat-7.14.1-linux-x86_64.tar.gz
```

adjust the filebeat.yml paths, my current directory structure are 
  * $HOME/var/logs/wfe-aws1-4/
  * $HOME/var/logs/wfe-aws1-5/
  * ... 
 
```
  paths:
    - /home/zcgg/var/logs/wfe*/var/log/apache/*.log

```

After completing the adjustment then run the following

```
./filebeat -e -c filebeat.yml
```

### !! NOTE !! 

 1. I usually run around 12-16 GiB of RAM with Oracle's JDK so if needed an adjustment do so
 1. You should've enough HDD space
 1. It'll take almost 1 hour to ingest 1 log file so be prepared also filebeat doesn't support `zcat` so it sucks that all log files should be extracted and 1 log file is almost 1 GiB.
 1. For the 3 random logs I created it ballooned to a whooping 2 TiB on an NVME PCIE gen 4 that was ran overnight.
 