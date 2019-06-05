usage: prometheus [<flags>]

The Prometheus monitoring server

Flags:
  -h, --help                     Hiển thị hướng dẫn chi tiết
      --version                  Hiển thị phiên bản phần mềm
      --config.file="prometheus.yml"
                                 Đường dẫn đền tệp cấu hình của Prometheus.
      --web.listen-address="0.0.0.0:9090"
                                 Địa chỉ lắng nghe cho UI, API và telemetry
      --web.read-timeout=5m      Thời lượng tối đa trước khi 
      --web.max-connections=512  Số lượng kết nối tối đa đồng thời.
      --web.external-url=<URL>   The URL under which Prometheus is externally reachable (for example, if Prometheus is served via a reverse proxy). Used for generating relative and absolute links back to Prometheus itself. If the URL has a path portion, it will be used to prefix all HTTP endpoints served by Prometheus. If omitted, relevant URL components will be derived automatically.
         
      --web.route-prefix=<path>  Prefix for the internal routes of web endpoints. Defaults to path of --web.extern
      --web.user-assets=<path>   Path to static asset directory, available at /user.
      --web.enable-lifecycle     Enable shutdown and reload via HTTP request.
      --web.enable-admin-api     Enable API endpoints for admin control actions.
      --web.console.templates="consoles"
                                 Path to the console template directory, available at /consoles.
      --web.console.libraries="console_libraries"
                                 Path to the console library directory.
      --web.page-title="Prometheus Time Series Collection and Processing Server"
                                 Document title of Prometheus instance.
      --web.cors.origin=".*"     Regex for CORS origin. It is fully anchored. Eg. 'https?://(domain1|domain2)\.com
      --storage.tsdb.path="data/"
                                 Base path for metrics storage.
      --storage.tsdb.retention=STORAGE.TSDB.RETENTION
                                 [DEPRECATED] How long to retain samples in storage. This flag has been deprecated
      --storage.tsdb.retention.time=STORAGE.TSDB.RETENTION.TIME
                                 How long to retain samples in storage. When this flag is set it overrides "storag
                                 "storage.tsdb.retention.size" is set, the retention time defaults to 15d.
      --storage.tsdb.retention.size=STORAGE.TSDB.RETENTION.SIZE
                                 [EXPERIMENTAL] Maximum number of bytes that can be stored for blocks. Units suppo
      --storage.tsdb.no-lockfile
                                 Do not create lockfile in data directory.
      --storage.tsdb.allow-overlapping-blocks
                                 [EXPERIMENTAL] Allow overlapping blocks which in-turn enables vertical compaction
      --storage.remote.flush-deadline=<duration>
                                 How long to wait flushing sample on shutdown or config reload.
      --storage.remote.read-sample-limit=5e7
                                 Maximum overall number of samples to return via the remote read interface, in a s
      --storage.remote.read-concurrent-limit=10
                                 Maximum number of concurrent remote read calls. 0 means no limit.
      --rules.alert.for-outage-tolerance=1h
                                 Max time to tolerate prometheus outage for restoring 'for' state of alert.
      --rules.alert.for-grace-period=10m
                                 Minimum duration between alert and restored 'for' state. This is maintained only
      --rules.alert.resend-delay=1m
                                 Minimum amount of time to wait before resending an alert to Alertmanager.
      --alertmanager.notification-queue-capacity=10000
                                 The capacity of the queue for pending Alertmanager notifications.
      --alertmanager.timeout=10s
                                 Timeout for sending alerts to Alertmanager.
      --query.lookback-delta=5m  The delta difference allowed for retrieving metrics during expression evaluations
      --query.timeout=2m         Maximum time a query may take before being aborted.
      --query.max-concurrency=20
                                 Maximum number of queries executed concurrently.
      --query.max-samples=50000000
                                 Maximum number of samples a single query can load into memory. Note that queries
                                 number of samples a query can return.
      --log.level=info           Only log messages with the given severity or above. One of: [debug, info, warn, e
      --log.format=logfmt        Output format of log messages. One of: [logfmt, json]
