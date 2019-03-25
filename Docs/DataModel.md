# Data Model

Prometheus lưu trữ tất cả dữ liệu theo chuỗi thời gian. Các luồng giá trị được đánh dấu thời gian(timestamped) thuộc cùng một số liệu và cùng một bộ nhãn. Bên cạnh việc lưu trữ time series, Prometheus có thể tạo chuỗi thời gian xuất phát tạm thời như là kết quả của chuỗi truy vấn.

_(*) time series là một chuỗi thời thời gian lưu trữ các dữ liệu được đo từ những thời điểm có tần xuất xác định_

## Metric names và labels
Mỗi time series được xác định là duy nhát bởi _metric name_ và một bộ key-value còn được gọi là _labels_.

_Metric name_ chỉ định tính năng chung của hệ thống được đo _(Ví dụ như http_requests_total - là tổng số các request HTTP nhận được )_.

_Labels_ cho phép các truy vấn lọc và tổng hợp. 

```sh
<metric name>{<label name>=<label value>, ...}
```

Ví dụ: _api_http_requests_total{method="POST", handler="/messages"}_

## Samples
Samples form gồm:
	* một giá trị float64
	* một timestamp được xác định chính xác đến mili giây.
