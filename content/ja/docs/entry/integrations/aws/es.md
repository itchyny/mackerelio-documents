---
Title: AWSインテグレーション - Elasticsearch Service
Date: 2019-01-18T18:00:00+09:00
URL: https://mackerel.io/ja/docs/entry/integrations/aws/es
EditURL: https://blog.hatena.ne.jp/mackerelio/mackerelio-docs-ja.hatenablog.mackerel.io/atom/entry/98012380832127355
---

MackerelはAWSインテグレーションにて<a href="https://aws.amazon.com/jp/elasticsearch-service/" target="_blank">Amazon Elasticsearch Service</a>のメトリック取得や監視に対応しています。
AWSインテグレーションで連携を行なった場合、課金対象として1ドメイン = 1マイクロホストと換算します。またそれに加えて、取得されるメトリックの数に応じて、1マイクロホストあたりのメトリック数上限の超過による請求が行われます。

AWSインテグレーションの設定方法や対応AWSサービス一覧についてはこちらのページをご確認ください。<br>
<a href="https://mackerel.io/ja/docs/entry/integrations/aws">AWSインテグレーション</a>

## 取得メトリック
AWSインテグレーションのElasticsearch Service対応で取得できるメトリックは以下の通りです。`メトリック`の説明に関しては<a href="https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html
" target="_blank">AWSのヘルプ</a>をご確認ください。

最大で72個のメトリックが取得されます。

|グラフ名|メトリック|Mackerel上のメトリック名|単位|Statistics|
|:--|:--|:--|:--|:--|
|Cluster Status|ClusterStatus.green<br>ClusterStatus.yellow<br>ClusterStatus.red|es.cluster_status.green<br>es.cluster_status.yellow<br>es.cluster_status.red|integer|Minimum<br>Maximum<br>Maximum|
|Nodes|Nodes|es.nodes.count|integer|Minimum|
|Searchable Documents|SearchableDocuments|es.searchable_documents.count|float|Average|
|Deleted Documents|DeletedDocuments|es.deleted_documents.count|float|Average|
|CPU Utilization|CPUUtilization|es.cpu_utilization.maximum<br>es.cpu_utilization.average|percentage|Maximum<br>Average|
|Free Storage Space|FreeStorageSpace|es.free_storage_space.maximum<br>es.free_storage_space.average<br>es.free_storage_space.minimum|bytes|Maximum<br>Average<br>Minimum|
|Cluster Used Space|ClusterUsedSpace|es.cluster_used_space.maximum<br>es.cluster_used_space.minimum|integer|Maximum<br>Minimum|
|Cluster Index Writes Blocked|ClusterIndexWritesBlocked|es.cluster_index_writes_blocked.blocked|integer|Maximum|
|JVM Memory Pressure|JVMMemoryPressure|es.jvm_memory_pressure.maximum|percentage|Maximum|
|Automated Snapshot Failure|AutomatedSnapshotFailure|es.automated_snapshot_failure.failure|integer|Maximum|
|Health Check|KibanaHealthyNodes<br>MasterReachableFromNode|es.health_check.kibana<br>es.health_check.master|integer|Minimum|
|KMS Key|KMSKeyError<br>KMSKeyInaccessible|es.kms_key.error<br>es.kms_key.inaccessible|integer|Maximum|
|Cluster Requests|ElasticsearchRequests<br>InvalidHostHeaderRequests|es.cluster_requests.count<br>es.cluster_requests.invalid_host_header|integer|Sum|
|Requests|2xx<br>3xx<br>4xx<br>5xx|es.requests.2xx<br>es.requests.3xx<br>es.requests.4xx<br>es.requests.5xx|integer|Sum|
|Master CPU Utilization|MasterCPUUtilization|es.master_cpu_utilization.master_cpu|percentage|Average|
|Master JVM Memory Pressure|MasterJVMMemoryPressure|es.master_jvm_memory_pressure.master_jvm|percentage|Maximum|
|EBS Disk Queue Depth|DiskQueueDepth|es.ebs_disk_queue_depth.minimum<br>es.ebs_disk_queue_depth.average<br>es.ebs_disk_queue_depth.maximum|float|Minimum<br>Average<br>Maximum|
|EBS Latency|WriteLatency<br>ReadLatency|es.ebs_latency.#.minimum<br>es.ebs_latency.#.average<br>es.ebs_latency.#.maximum|float|Minimum<br>Average<br>Maximum|
|EBS Throughput|WriteThroughput<br>ReadThroughput|es.ebs_throughput.#.minimum<br>es.ebs_throughput.#.average<br>es.ebs_throughput.#.maximum|bytes/sec|Minimum<br>Average<br>Maximum|
|EBS IOPS|WriteIOPS<br>ReadIOPS|es.ebs_iops.#.minimum<br>es.ebs_iops.#.average<br>es.ebs_iops.#.maximum|iops|Minimum<br>Average<br>Maximum|
|Instance Latency|IndexingLatency<br>SearchLatency|es.instance_latency.indexing<br>es.instance_latency.search|float|Average|
|Instance Rate|IndexingRate<br>SearchRate|es.instance_rate.indexing<br>es.instance_rate.search|float|Average|
|Instance Sys Memory Utilization|SysMemoryUtilization|es.instance_sys_memory_utilization.minimum<br>es.instance_sys_memory_utilization.average<br>es.instance_sys_memory_utilization.maximum|percentage|Minimum<br>Average<br>Maximum|
|JVM GC Collection Count|JVMGCYoungCollectionCount<br>JVMGCOldCollectionCount|es.jvm_gc_collection_count.young<br>es.jvm_gc_collection_count.old|integer|Maximum|
|JVM GC Collection Time|JVMGCYoungCollectionTime<br>JVMGCOldCollectionTime|es.jvm_gc_collection_time.young<br>es.jvm_gc_collection_time.old|integer|Maximum|
|Threadpool Index Threads|ThreadpoolIndexQueue<br>ThreadpoolIndexRejected<br>ThreadpoolIndexThreads|es.threadpool_index_threads.queue<br>es.threadpool_index_threads.rejected<br>es.threadpool_index_threads.threads|integer|Maximum|
|Threadpool Search Threads|ThreadpoolSearchQueue<br>ThreadpoolSearchRejected<br>ThreadpoolSearchThreads|es.threadpool_search_threads.queue<br>es.threadpool_search_threads.rejected<br>es.threadpool_search_threads.threads|integer|Maximum|
|Threadpool Force_merge Threads|ThreadpoolForce_mergeQueue<br>ThreadpoolForce_mergeRejected<br>ThreadpoolForce_mergeThreads|es.threadpool_force_merge_threads.queue<br>es.threadpool_force_merge_threads.rejected<br>es.threadpool_force_merge_threads.threads|integer|Maximum|
|Threadpool Bulk Threads|ThreadpoolBulkQueue<br>ThreadpoolBulkRejected<br>ThreadpoolBulkThreads|es.threadpool_bulk_threads.queue<br>es.threadpool_bulk_threads.rejected<br>es.threadpool_bulk_threads.threads|integer|Maximum|

- "Mackerel上のメトリック名"の#には"Write"、"Read"のいずれかが入ります。

<h2 id="notes">注意事項</h2>

AWSインテグレーションにより取得可能な上記のグラフ・メトリックのうち、下記のグラフに含まれるメトリックについては、メトリックの取得間隔が異なります。

- 5分間隔
  - Cluster Index Writes Blocked
  - Request
  - EBS Latency
  - EBS Throughput
  - EBS IOPS
- 1時間間隔
  - KMS Key
- 発生ごと
  - Cluster Requests
