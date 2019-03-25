# Prometheus

## Tổng quan

Prometheus là một nền tảng mã nguồn mở Monitoring thuộc tổ chức Cloud Native Computing Foudation project, là một hệ thống và dịch vụ monitoring system. Nó thu nhập những metrics từ những node mà nó quan sát, đánh giá các dữ liệu, hiển thị kết quả và nó có thể kích hoạt những cảnh báo nếu hệ thống có thể gặp vấn đề.

![Prometheus Logo](./Images/logo.png)

Các tính năng phân biệt chính của Prometheus so với các hệ thống giám sát khác là:
	* Mô hình dữ liệu đa chiều _(thời gian được xác định bởi tên số liệu và bộ key/value)_
	* Một ngôn ngữ truy vấn linh hoạt để tận dụng chiều hướng này.
	* Không phụ thuộc vào lưu trữ phân tán, chỉ cần một server duy nhất tự động.
	* Chuỗi thời gian thu nhập xảy ra thông qua một mô hình pull dựa vào http.
	* Nhiều plugin hỗ trợ từ bên thứ 3.
	* Hỗ trợ Push các timeseries thông qua một gateway trung gian.
	* Targets are discovered via service discovery or static configuration.
	* Nhiều chế độ hỗ trợ vẽ đồ thị và bảng điều khiển.
	* Hỗ trợ cho liên kết phân cấp ngang.

## Đặc điểm về Prometheus.
	* Là phần mềm mã nguồn mở. Được viết bằng ngôn ngữ Go _(chủ yếu)_ , Java, Python và Ruby.
	* Prometheus chủ yếu yêu cầu các máy remote gửi các thông số. Ngoài ra cũng có lúc remote chủ động push nếu sử dụng PushGateway.
	* Sử dụng Grafana giúp Prometheus thêm phần trực quan hơn.

## Kiến trúc Prometheus
Prometheus gồm những thành phần sau:

	* **Prometheus server** là thành phần chính của hệ thống - cái mà cảnh báo và lưu trữ dữ liệu theo thời gian.
	* **client libraries** thư viện code các plugin.
	* Một Push Gateway cho hỗ trợ các công việc ngắn hạn.
	* **Exporter** thu nhập dữ liệu metric từ các service HAProxy, StatsD, Graphite,...
	* **Alertmanager** là thành phần quản lí cảnh bảo và gửi cảnh báo,....

![Kiến trúc Prometheus](./Images/kientruc.png)
