---
title: Device Logs
---

# {{ title }}

Device logging and the storage of device logs in {{ $names.cloud.lower }} is designed to be a debugging feature. The Logs section can be used to view and download logs from the system and app services running on the device in real time.

<img alt="Dashboard Logs" src="/ADD a photo.png">


## Device logs on the balenaCloud dashboard

Device logs on the balenaCloud dashboard contains anything written to stdout and stderr of app services, and the system logs of the device. The dashboard allows logs to be cleared, filtered, searched and viewed according to the browser timezone (or UTC). 

1000 lines is the number of lines of logs stored in the API, viewable from the dashboard and available for download. Device logging in balenaCloud isn't meant to be used for long-term, reliable storage of logs. It's instead designed to provide the latest logs from the device for quick debugging purposes. We do plan on expanding our logging solution to offer long-term storage and search.

## Persistent logging

The ability to read logs from the different services that make up balenaOS is vital in helping to track down issues that arise. However, on reboot these logs will be cleared and so examining them will not, for example, give any insight as to why the reboot may have occurred (or which services may have failed causing a reboot). To alleviate this, balenaOS allows persistent logs to be enabled by customers.  

Persistent logging can be enabled using the Configuration tab on the sidebar for either specific using device configuration or fleet wide. Once there, select 'Activate' on 'Enable persistent logging'. The device(s) will reboot once persistent logging is activated to ensure that the settings are applied. Once enabled, they end up stored as part of the data partition on the device (either on SD card, eMMC, harddisk, etc.). This is located on-device at /var/log/journal/<uuid> where the UUID is variable.

Depending on the OS version, the size of persistent logs can be increased to store more logs than the default size (32 MB currently). This also increases the wear on the storage medium of device. Refer to [long term storage of device logs](#long-term-device-logs-storage) for ways to offset this.

The `RuntimeMaxUse=` in `/etc/systemd/journald.conf` can be increased in order to potentially increase the amount of logs being stored. Refer to [journald.conf docs](https://www.freedesktop.org/software/systemd/man/journald.conf.html) for more information regaridng the same.

## Long term device logs storage

If you are dealing with lot of logs over the course of time, then persistent logging might not be a reliable long term solution. Persistent logging results in increased writes on the storage media of the device which leads to your storage media degrading incrementally over time.

Instead, device logs can be streamed using the supervisor API. Refer to the [Supervisor's journald](https://www.balena.io/docs/reference/supervisor/supervisor-api/#journald-logs) endpoint. Through this endpoint, device logs can be streamed to whichever cloud monitoring platform you use to store logs reliably over the course of time. 

For example, Datadog: https://www.balena.io/blog/iot-fleet-monitoring-with-datadog-and-balenacloud-how-small-agent-containers-make-a-big-impact/
