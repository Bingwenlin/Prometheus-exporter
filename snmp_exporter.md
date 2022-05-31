### prometheus监控交换机-配置文件snmp.yml
```yml
# WARNING: This file was auto-generated using snmp_exporter generator, manual changes will be lost.
if_mib:
  walk:
  - 1.3.6.1.2.1.2.2.1.10
  - 1.3.6.1.2.1.2.2.1.16
  - 1.3.6.1.2.1.2.2.1.8
  get:
  - 1.3.6.1.2.1.1.3.0
  metrics:
  - name: sysUpTime
    oid: 1.3.6.1.2.1.1.3
    type: gauge
    help: The time (in hundredths of a second) since the network management portion
      of the system was last re-initialized. - 1.3.6.1.2.1.1.3
  - name: ifInOctets
    oid: 1.3.6.1.2.1.2.2.1.10
    type: counter
    help: The total number of octets received on the interface, including framing
      characters - 1.3.6.1.2.1.2.2.1.10
    indexes:
    - labelname: ifIndex
      type: gauge
  - name: ifOutOctets
    oid: 1.3.6.1.2.1.2.2.1.16
    type: counter
    help: The total number of octets transmitted out of the interface, including framing
      characters - 1.3.6.1.2.1.2.2.1.16
    indexes:
    - labelname: ifIndex
      type: gauge
  - name: ifOperStatus
    oid: 1.3.6.1.2.1.2.2.1.8
    type: gauge
    help: The current operational state of the interface - 1.3.6.1.2.1.2.2.1.8
    indexes:
    - labelname: ifIndex
      type: gauge
    enum_values:
      1: up
      2: down
      3: testing
      4: unknown
      5: dormant
      6: notPresent
      7: lowerLayerDown
  version: 2
  auth:
    community: zabbix@123

```

#### 安装包 ![[snmp_exporter.tar.gz]]
