# splunk-detections

Splunk searches and a dashboard for catching SSH brute force and odd logins on my home lab VM (lab-vm, Ubuntu 22.04 running SSH and Apache).

Still working on it.

## How I run Splunk
Splunk runs in Docker (the free trial image, version 10.6.1.1 when I did this). The admin password comes from the SPLUNK_PASSWORD environment variable so it's not in any file.

```
docker run -d --name splunk -p 8000:8000 \
  -e TZ=America/Toronto \
  -e SPLUNK_GENERAL_TERMS=--accept-sgt-current-at-splunk-com \
  -e SPLUNK_START_ARGS=--accept-license \
  -e SPLUNK_PASSWORD \
  -v "$(pwd)/../splunk-lab/logs:/var/log/lab:ro" \
  -v "$(pwd)/config:/opt/splunk/etc/apps/lab_inputs/local" \
  splunk/splunk:latest
```

The VM's /var/log/auth.log and /var/log/apache2/access.log get copied live into the logs folder with tail -F, and config/inputs.conf tells Splunk to read them. config/props.conf sets the timezone for auth.log, because its lines don't have one and Splunk would read them as UTC.
