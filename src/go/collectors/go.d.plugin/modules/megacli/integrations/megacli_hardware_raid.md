<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/src/go/collectors/go.d.plugin/modules/megacli/README.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/src/go/collectors/go.d.plugin/modules/megacli/metadata.yaml"
sidebar_label: "MegaCli Hardware Raid"
learn_status: "Published"
learn_rel_path: "Collecting Metrics/Storage, Mount Points and Filesystems"
most_popular: False
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE COLLECTOR'S metadata.yaml FILE"
endmeta-->

# MegaCli Hardware Raid


<img src="https://netdata.cloud/img/hard-drive.svg" width="150"/>


Plugin: go.d.plugin
Module: megacli

<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Overview

Monitors the health of MegaCLI Hardware RAID by tracking the status of RAID adapters, physical drives, and backup batteries in your storage system.
It relies on the `megacli` CLI tool but avoids directly executing the binary.
Instead, it utilizes `ndsudo`, a Netdata helper specifically designed to run privileged commands securely within the Netdata environment.
This approach eliminates the need to use `sudo`, improving security and potentially simplifying permission management.

Executed commands:
-  `megacli -LDPDInfo -aAll -NoLog`
-  `megacli -AdpBbuCmd -aAll -NoLog`




This collector is supported on all platforms.

This collector only supports collecting metrics from a single instance of this integration.


### Default Behavior

#### Auto-Detection

This integration doesn't support auto-detection.

#### Limits

The default configuration for this integration does not impose any limits on data collection.

#### Performance Impact

The default configuration for this integration is not expected to impose a significant performance impact on the system.


## Metrics

Metrics grouped by *scope*.

The scope defines the instance that the metric belongs to. An instance is uniquely identified by a set of labels.



### Per adapter

These metrics refer to the MegaCLI Adapter.

Labels:

| Label      | Description     |
|:-----------|:----------------|
| adapter_number | Adapter number |

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| megacli.adapter_health_state | optimal, degraded, partially_degraded, failed | state |

### Per physical drive

These metrics refer to the MegaCLI Physical Drive.

Labels:

| Label      | Description     |
|:-----------|:----------------|
| adapter_number | Adapter number |
| wwn | World Wide Name |
| slot_number | Slot number |
| drive_position | Position (e.g. DiskGroup: 0, Span: 0, Arm: 2) |
| drive_type | Type (e.g. SATA) |

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| megacli.phys_drive_media_errors_rate | media_errors | errors/s |
| megacli.phys_drive_predictive_failures_rate | predictive_failures | failures/s |

### Per backup battery unit

These metrics refer to the MegaCLI Backup Battery Unit.

Labels:

| Label      | Description     |
|:-----------|:----------------|
| adapter_number | Adapter number |
| battery_type | Battery type (e.g. BBU) |

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| megacli.bbu_relative_charge | charge | percentage |
| megacli.bbu_recharge_cycles | recharge | cycles |
| megacli.bbu_temperature | temperature | Celsius |



## Alerts

There are no alerts configured by default for this integration.


## Setup

### Prerequisites

No action required.

### Configuration

#### File

The configuration file name for this integration is `go.d/megacli.conf`.


You can edit the configuration file using the `edit-config` script from the
Netdata [config directory](https://github.com/netdata/netdata/blob/master/docs/netdata-agent/configuration.md#the-netdata-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config go.d/megacli.conf
```
#### Options

The following options can be defined globally: update_every.


<details><summary>Config options</summary>

| Name | Description | Default | Required |
|:----|:-----------|:-------|:--------:|
| update_every | Data collection frequency. | 10 | no |
| timeout | lvs binary execution timeout. | 2 | no |

</details>

#### Examples

##### Custom update_every

Allows you to override the default data collection interval.

<details><summary>Config</summary>

```yaml
jobs:
  - name: megacli
    update_every: 5  # Collect MegaCli Hardware RAID statistics every 5 seconds

```
</details>



## Troubleshooting

### Debug Mode

To troubleshoot issues with the `megacli` collector, run the `go.d.plugin` with the debug option enabled. The output
should give you clues as to why the collector isn't working.

- Navigate to the `plugins.d` directory, usually at `/usr/libexec/netdata/plugins.d/`. If that's not the case on
  your system, open `netdata.conf` and look for the `plugins` setting under `[directories]`.

  ```bash
  cd /usr/libexec/netdata/plugins.d/
  ```

- Switch to the `netdata` user.

  ```bash
  sudo -u netdata -s
  ```

- Run the `go.d.plugin` to debug the collector:

  ```bash
  ./go.d.plugin -d -m megacli
  ```

