<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/src/go/plugin/go.d/modules/snmp/README.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/src/go/plugin/go.d/modules/snmp/metadata.yaml"
sidebar_label: "SNMP devices"
learn_status: "Published"
learn_rel_path: "Collecting Metrics/Generic Collecting Metrics"
most_popular: True
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE COLLECTOR'S metadata.yaml FILE"
endmeta-->

# SNMP devices


<img src="https://netdata.cloud/img/snmp.png" width="150"/>


Plugin: go.d.plugin
Module: snmp

<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Overview

This SNMP collector discovers and gathers statistics for network interfaces on SNMP-enabled devices:

- Traffic
- Packets (unicast, multicast, broadcast)
- Errors
- Discards
- Administrative and operational status

Additionally, it collects overall device uptime.

It is compatible with all SNMP versions (v1, v2c, and v3) and uses the [gosnmp](https://github.com/gosnmp/gosnmp) package.

**For advanced users**:

- You can manually specify custom OIDs (Object Identifiers) to retrieve specific data points beyond the default metrics.
- However, defining custom charts with dimensions for these OIDs requires manual configuration.




This collector is supported on all platforms.

This collector supports collecting metrics from multiple instances of this integration, including remote instances.


### Default Behavior

#### Auto-Detection

This integration doesn't support auto-detection.

#### Limits

The default configuration for this integration does not impose any limits on data collection.

#### Performance Impact

**Device limitations**: Many SNMP switches and routers have limited processing power. They might not be able to report data as frequently as desired. You can monitor response times using go.d.plugin in debug mode to identify potential bottlenecks.

**Concurrent access**: If multiple collectors or tools access the same SNMP device simultaneously, data points might be skipped. This is a limitation of the device itself, not this collector. To mitigate this, consider increasing the collection interval (update_every) to reduce the frequency of requests.



## Metrics

Metrics grouped by *scope*.

The scope defines the instance that the metric belongs to. An instance is uniquely identified by a set of labels.

The metrics that will be collected are defined in the configuration file.

### Per snmp device

These metrics refer to the SNMP device.

Labels:

| Label      | Description     |
|:-----------|:----------------|
| sysName | SNMP device's system name (OID: [1.3.6.1.2.1.1.5](https://oidref.com/1.3.6.1.2.1.1.5)). |

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| snmp.device_uptime | uptime | seconds |

### Per network interface

Network interfaces of the SNMP device being monitored. These metrics refer to each interface.

Labels:

| Label      | Description     |
|:-----------|:----------------|
| sysName | SNMP device's system name (OID: [1.3.6.1.2.1.1.5](https://oidref.com/1.3.6.1.2.1.1.5)). |
| ifDescr | Network interface description (OID: [1.3.6.1.2.1.2.2.1.2](https://cric.grenoble.cnrs.fr/Administrateurs/Outils/MIBS/?oid=1.3.6.1.2.1.2.2.1.2)). |
| ifName | Network interface name (OID: [1.3.6.1.2.1.2.2.1.2](https://cric.grenoble.cnrs.fr/Administrateurs/Outils/MIBS/?oid=1.3.6.1.2.1.31.1.1.1.1)). |
| ifType | Network interface type (OID: [1.3.6.1.2.1.2.2.1.2](https://cric.grenoble.cnrs.fr/Administrateurs/Outils/MIBS/?oid=1.3.6.1.2.1.2.2.1.3)). |

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| snmp.device_net_interface_traffic | received, sent | kilobits/s |
| snmp.device_net_interface_unicast | received, sent | packets/s |
| snmp.device_net_interface_multicast | received, sent | packets/s |
| snmp.device_net_interface_broadcast | received, sent | packets/s |
| snmp.device_net_interface_errors | inbound, outbound | errors/s |
| snmp.device_net_interface_discards | inbound, outbound | discards/s |
| snmp.device_net_interface_admin_status | up, down, testing | status |
| snmp.device_net_interface_oper_status | up, down, testing, unknown, dormant, not_present, lower_layer_down | status |



## Alerts

There are no alerts configured by default for this integration.


## Setup

### Prerequisites

No action required.

### Configuration

#### File

The configuration file name for this integration is `go.d/snmp.conf`.


You can edit the configuration file using the `edit-config` script from the
Netdata [config directory](/docs/netdata-agent/configuration/README.md#the-netdata-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config go.d/snmp.conf
```
#### Options

The following options can be defined globally: update_every, autodetection_retry.


<details open><summary>Config options</summary>

| Name | Description | Default | Required |
|:----|:-----------|:-------|:--------:|
| update_every | Data collection frequency. | 10 | no |
| autodetection_retry | Recheck interval in seconds. Zero means no recheck will be scheduled. | 0 | no |
| hostname | Target ipv4 address. |  | yes |
| community | SNMPv1/2 community string. | public | no |
| options.version | SNMP version. Available versions: 1, 2, 3. | 2 | no |
| options.port | Target port. | 161 | no |
| options.retries | Retries to attempt. | 1 | no |
| options.timeout | SNMP request/response timeout. | 5 | no |
| options.max_repetitions | Controls how many SNMP variables to retrieve in a single GETBULK request. | 25 | no |
| options.max_request_size | Maximum number of OIDs allowed in a single GET request. | 60 | no |
| network_interface_filter.by_name | Filter interfaces by their names using [simple patterns](/src/libnetdata/simple_pattern/README.md#simple-patterns). |  | no |
| network_interface_filter.by_type | Filter interfaces by their types using [simple patterns](/src/libnetdata/simple_pattern/README.md#simple-patterns). |  | no |
| user.name | SNMPv3 user name. |  | no |
| user.name | Security level of SNMPv3 messages. |  | no |
| user.auth_proto | Security level of SNMPv3 messages. |  | no |
| user.name | Authentication protocol for SNMPv3 messages. |  | no |
| user.auth_key | Authentication protocol pass phrase. |  | no |
| user.priv_proto | Privacy protocol for SNMPv3 messages. |  | no |
| user.priv_key | Privacy protocol pass phrase. |  | no |
| charts | List of charts. | [] | yes |
| charts.id | Chart ID. Used to uniquely identify the chart. |  | yes |
| charts.title | Chart title. | Untitled chart | no |
| charts.units | Chart units. | num | no |
| charts.family | Chart family. | charts.id | no |
| charts.type | Chart type (line, area, stacked). | line | no |
| charts.priority | Chart priority. | 70000 | no |
| charts.multiply_range | Used when you need to define many charts using incremental OIDs. | [] | no |
| charts.dimensions | List of chart dimensions. | [] | yes |
| charts.dimensions.oid | Collected metric OID. |  | yes |
| charts.dimensions.name | Dimension name. |  | yes |
| charts.dimensions.algorithm | Dimension algorithm (absolute, incremental). | absolute | no |
| charts.dimensions.multiplier | Collected value multiplier, applied to convert it properly to units. | 1 | no |
| charts.dimensions.divisor | Collected value divisor, applied to convert it properly to units. | 1 | no |

##### user.auth_proto

The security of an SNMPv3 message as per RFC 3414 (`user.level`):

| String value | Int value | Description                              |
|:------------:|:---------:|------------------------------------------|
|     none     |     1     | no message authentication or encryption  |
|  authNoPriv  |     2     | message authentication and no encryption |
|   authPriv   |     3     | message authentication and encryption    |


##### user.name

The digest algorithm for SNMPv3 messages that require authentication (`user.auth_proto`):

| String value | Int value | Description                               |
|:------------:|:---------:|-------------------------------------------|
|     none     |     1     | no message authentication                 |
|     md5      |     2     | MD5 message authentication (HMAC-MD5-96)  |
|     sha      |     3     | SHA message authentication (HMAC-SHA-96)  |
|    sha224    |     4     | SHA message authentication (HMAC-SHA-224) |
|    sha256    |     5     | SHA message authentication (HMAC-SHA-256) |
|    sha384    |     6     | SHA message authentication (HMAC-SHA-384) |
|    sha512    |     7     | SHA message authentication (HMAC-SHA-512) |


##### user.priv_proto

The encryption algorithm for SNMPv3 messages that require privacy (`user.priv_proto`):

| String value | Int value | Description                                                             |
|:------------:|:---------:|-------------------------------------------------------------------------|
|     none     |     1     | no message encryption                                                   |
|     des      |     2     | ES encryption (CBC-DES)                                                 |
|     aes      |     3     | 128-bit AES encryption (CFB-AES-128)                                    |
|    aes192    |     4     | 192-bit AES encryption (CFB-AES-192) with "Blumenthal" key localization |
|    aes256    |     5     | 256-bit AES encryption (CFB-AES-256) with "Blumenthal" key localization |
|   aes192c    |     6     | 192-bit AES encryption (CFB-AES-192) with "Reeder" key localization     |
|   aes256c    |     7     | 256-bit AES encryption (CFB-AES-256) with "Reeder" key localization     |


</details>

#### Examples

##### SNMPv1/2

In this example:

- the SNMP device is `192.0.2.1`.
- the SNMP version is `2`.
- the SNMP community is `public`.
- we will update the values every 10 seconds.


<details open><summary>Config</summary>

```yaml
jobs:
  - name: switch
    update_every: 10
    hostname: 192.0.2.1
    community: public
    options:
      version: 2

```
</details>

##### SNMPv3

To use SNMPv3:

- use `user` instead of `community`.
- set `options.version` to 3.


<details open><summary>Config</summary>

```yaml
jobs:
  - name: switch
    update_every: 10
    hostname: 192.0.2.1
    options:
      version: 3
    user:
      name: username
      level: authPriv
      auth_proto: sha256
      auth_key: auth_protocol_passphrase
      priv_proto: aes256
      priv_key: priv_protocol_passphrase

```
</details>

##### Custom OIDs

In this example:

- the SNMP device is `192.0.2.1`.
- the SNMP version is `2`.
- the SNMP community is `public`.
- we will update the values every 10 seconds.


<details open><summary>Config</summary>

```yaml
jobs:
  - name: switch
    update_every: 10
    hostname: 192.0.2.1
    community: public
    options:
      version: 2
    charts:
      - id: "bandwidth_port1"
        title: "Switch Bandwidth for port 1"
        units: "kilobits/s"
        type: "area"
        family: "ports"
        dimensions:
          - name: "in"
            oid: "1.3.6.1.2.1.2.2.1.10.1"
            algorithm: "incremental"
            multiplier: 8
            divisor: 1000
          - name: "out"
            oid: "1.3.6.1.2.1.2.2.1.16.1"
            multiplier: -8
            divisor: 1000
      - id: "bandwidth_port2"
        title: "Switch Bandwidth for port 2"
        units: "kilobits/s"
        type: "area"
        family: "ports"
        dimensions:
          - name: "in"
            oid: "1.3.6.1.2.1.2.2.1.10.2"
            algorithm: "incremental"
            multiplier: 8
            divisor: 1000
          - name: "out"
            oid: "1.3.6.1.2.1.2.2.1.16.2"
            multiplier: -8
            divisor: 1000

```
</details>

##### Custom OIDs with multiply range

If you need to define many charts using incremental OIDs, you can use the `charts.multiply_range` option.

This is like the SNMPv1/2 example, but the option will multiply the current chart from 1 to 24 inclusive, producing 24 charts in total for the 24 ports of the switch `192.0.2.1`.

Each of the 24 new charts will have its id (1-24) appended at:

- its chart unique `id`, i.e. `bandwidth_port_1` to `bandwidth_port_24`.
- its title, i.e. `Switch Bandwidth for port 1` to `Switch Bandwidth for port 24`.
- its `oid` (for all dimensions), i.e. dimension in will be `1.3.6.1.2.1.2.2.1.10.1` to `1.3.6.1.2.1.2.2.1.10.24`.
- its `priority` will be incremented for each chart so that the charts will appear on the dashboard in this order.


<details open><summary>Config</summary>

```yaml
jobs:
  - name: switch
    update_every: 10
    hostname: "192.0.2.1"
    community: public
    options:
      version: 2
    charts:
      - id: "bandwidth_port"
        title: "Switch Bandwidth for port"
        units: "kilobits/s"
        type: "area"
        family: "ports"
        multiply_range: [1, 24]
        dimensions:
          - name: "in"
            oid: "1.3.6.1.2.1.2.2.1.10"
            algorithm: "incremental"
            multiplier: 8
            divisor: 1000
          - name: "out"
            oid: "1.3.6.1.2.1.2.2.1.16"
            multiplier: -8
            divisor: 1000

```
</details>

##### Multiple devices with a common configuration

YAML supports [anchors](https://yaml.org/spec/1.2.2/#3222-anchors-and-aliases). 
The `&` defines and names an anchor, and the `*` uses it. `<<: *anchor` means, inject the anchor, then extend. We can use anchors to share the common configuration for multiple devices.

The following example:

- adds an `anchor` to the first job.
- injects (copies) the first job configuration to the second and updates `name` and `hostname` parameters.
- injects (copies) the first job configuration to the third and updates `name` and `hostname` parameters.


<details open><summary>Config</summary>

```yaml
jobs:
  - &anchor
    name: switch
    update_every: 10
    hostname: "192.0.2.1"
    community: public
    options:
      version: 2
    charts:
      - id: "bandwidth_port1"
        title: "Switch Bandwidth for port 1"
        units: "kilobits/s"
        type: "area"
        family: "ports"
        dimensions:
          - name: "in"
            oid: "1.3.6.1.2.1.2.2.1.10.1"
            algorithm: "incremental"
            multiplier: 8
            divisor: 1000
          - name: "out"
            oid: "1.3.6.1.2.1.2.2.1.16.1"
            multiplier: -8
            divisor: 1000
  - <<: *anchor
    name: switch2
    hostname: "192.0.2.2"
  - <<: *anchor
    name: switch3
    hostname: "192.0.2.3"

```
</details>



## Troubleshooting

### Debug Mode

To troubleshoot issues with the `snmp` collector, run the `go.d.plugin` with the debug option enabled. The output
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
  ./go.d.plugin -d -m snmp
  ```

### Getting Logs

If you're encountering problems with the `snmp` collector, follow these steps to retrieve logs and identify potential issues:

- **Run the command** specific to your system (systemd, non-systemd, or Docker container).
- **Examine the output** for any warnings or error messages that might indicate issues.  These messages should provide clues about the root cause of the problem.

#### System with systemd

Use the following command to view logs generated since the last Netdata service restart:

```bash
journalctl _SYSTEMD_INVOCATION_ID="$(systemctl show --value --property=InvocationID netdata)" --namespace=netdata --grep snmp
```

#### System without systemd

Locate the collector log file, typically at `/var/log/netdata/collector.log`, and use `grep` to filter for collector's name:

```bash
grep snmp /var/log/netdata/collector.log
```

**Note**: This method shows logs from all restarts. Focus on the **latest entries** for troubleshooting current issues.

#### Docker Container

If your Netdata runs in a Docker container named "netdata" (replace if different), use this command:

```bash
docker logs netdata 2>&1 | grep snmp
```

