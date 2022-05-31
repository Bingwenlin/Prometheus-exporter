# cloudwatch_exporter

[[github]]https://github.com/ivx/yet-another-cloudwatch-exporter

### 下载二进制文件yace
```
wget https://github.com/ivx/yet-another-cloudwatch-exporter/releases/download/v0.27.0-alpha/yet-another-cloudwatch-exporter_0.27.0-alpha_Linux_x86_64.tar.gz

```
### 执行yace
```
./yace

```
### 配置config.yml文件
```
discovery:
  jobs:
  - type: ec
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-redis-primary
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: SwapUsage
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesIn
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesOut
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: Evictions
        statistics:
        - Average
        period: 600
        length: 60
      - name: Reclaimed
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationBytes
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationLag
        statistics:
        - Average
        period: 600
        length: 60
      - name: BytesUsedForCache
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrItems
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseMemoryUsagePercentage
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasMisses
        statistics:
        - Average
        period: 600
        length: 60
  - type: ec
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-redis-replica
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseMemoryUsagePercentage
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: SwapUsage
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesIn
        statistics:
        - Average
        period: 600
        length: 60
      - name: NetworkBytesOut
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: Evictions
        statistics:
        - Average
        period: 600
        length: 60
      - name: Reclaimed
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationBytes
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReplicationLag
        statistics:
        - Average
        period: 600
        length: 60
      - name: BytesUsedForCache
        statistics:
        - Average
        period: 600
        length: 60
      - name: CurrItems
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasHits
        statistics:
        - Average
        period: 600
        length: 60
      - name: CasMisses
        statistics:
        - Average
        period: 600
        length: 60
  - type: rds
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-mysql
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 60
  - type: rds
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-mysql-r
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 60
  - type: rds
    regions:
      - us-west-2
    searchTags:
      - Key: name
        Value: shoplus-data-analyze
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 60
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 60
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 60
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 60
  - type: cloudfront
    regions:
      - us-east-1
    metrics:
      - name: Requests
        statistics:
        - Sum
        period: 600
        length: 300
      - name: TotalErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 4xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 5xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300

```
### 第二种配置config.yml文件
```
discovery:
  jobs:
  - type: ec
    regions:
      - us-west-2
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 300
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 300
      - name: SwapUsage
        statistics:
        - Average
        period: 600
        length: 300
      - name: NetworkBytesIn
        statistics:
        - Average
        period: 600
        length: 300
      - name: NetworkBytesOut
        statistics:
        - Average
        period: 600
        length: 300
      - name: CurrConnections
        statistics:
        - Average
        period: 600
        length: 300
      - name: Evictions
        statistics:
        - Average
        period: 600
        length: 300
      - name: Reclaimed
        statistics:
        - Average
        period: 600
        length: 300
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 300
      - name: CacheHits
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReplicationBytes
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReplicationLag
        statistics:
        - Average
        period: 600
        length: 300
      - name: BytesUsedForCache
        statistics:
        - Average
        period: 600
        length: 300
      - name: CurrItems
        statistics:
        - Average
        period: 600
        length: 300
      - name: CasHits
        statistics:
        - Average
        period: 600
        length: 300
      - name: DatabaseMemoryUsagePercentage
        statistics:
        - Average
        period: 600
        length: 300
      - name: CasMisses
        statistics:
        - Average
        period: 600
        length: 300
  - type: rds
    regions:
      - us-west-2
    metrics:
      - name: CPUUtilization
        statistics:
        - Average
        period: 600
        length: 300
      - name: DatabaseConnections
        statistics:
        - Average
        period: 600
        length: 300
      - name: FreeStorageSpace
        statistics:
        - Average
        period: 600
        length: 300
      - name: FreeableMemory
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReadThroughput
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReadIOPS
        statistics:
        - Average
        period: 600
        length: 300
      - name: WriteLatency
        statistics:
        - Average
        period: 600
        length: 300
      - name: WriteThroughput
        statistics:
        - Average
        period: 600
        length: 300
      - name: WriteIOPS
        statistics:
        - Average
        period: 600
        length: 300
      - name: ReplicaLag
        statistics:
        - Average
        period: 600
        length: 300
  - type: cloudfront
    regions:
      - us-east-1
    metrics:
      - name: Requests
        statistics:
        - Sum
        period: 600
        length: 300
      - name: TotalErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 4xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: 5xxErrorRate
        statistics:
        - Average
        period: 600
        length: 300
      - name: BytesDownloaded
        statistics:
        - Sum
        period: 600
        length: 300
      - name: BytesUploaded
        statistics:
        - Sum
        period: 600
        length: 300

```